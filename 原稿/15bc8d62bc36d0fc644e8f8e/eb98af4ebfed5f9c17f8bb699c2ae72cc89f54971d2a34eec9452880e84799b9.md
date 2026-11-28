## 前言

记录一下 vscode-languageclient 配合 TowerLSP 如何实现自定义指令发送。

方法一：使用 `workspace/executeCommand`，允许 rust 端使用异步函数。

方法二：使用自定义注册消息，不允许 rust 端使用异步函数。

---

## 版本

使用编辑器： vscode 1.96.0

LSP客户端：

```json
"vscode-languageclient": "^9.0.1"
```

LSP服务端：

```toml
tower-lsp = { version = "0.20.0", features = ["proposed"]}
```

---

## 方法一：workspace/executeCommand

LSP 保留 workspace/executeCommand 这个字段用于执行自定义指令。

### 客户端使用

这里以代码诊断的部分为例，客户端可以如此来调用自定义指令获取后端提供的诊断信息

```typescript
export async function publishDiagnostics(
    client: LanguageClient,
    path: string
) {
    // 找到所有的库前缀，进行诊断（用于 verilator）
    await client.sendRequest("workspace/executeCommand", {
        command: 'publish-diagnostics',
        arguments: [path]
    });
}
```



### 服务端使用

上面的 `publish-diagnostics` 的后端实现如下

```rust
pub struct Backend {}

#[tower_lsp::async_trait]
impl LanguageServer for Backend {
    async fn execute_command(&self, params: ExecuteCommandParams) -> Result<Option<serde_json::Value>> {
        match params.command.as_str() {
            "publish-diagnostics" => {
                diagnostics::publish_diagnostics(backend, params.arguments).await
            }

            "clear-diagnostics" => {
                diagnostics::clear_diagnostics(backend, params.arguments).await
            }
			
            // 其他指令
            _ => {
                Ok(None)
            }
        }
    }
}
```

---

## 方法二：自定义消息

LSP 也允许用户自己定义消息来源，TowerLSP 通过一套比较复杂的方式可以实现。

### 客户端

假设我们自定义的消息为 `custom/paramRequest`，客户端如下调用

```typescript
import { LanguageClient, RequestType, ProtocolConnection } from 'vscode-languageclient/node';

export interface ICommonParam {
    param: string
}

// ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ 👇 输入参数，输出参数，回调函数 ~~~ 👇该消息的名称全局唯一，类似于 http 里面的路由 path
export const CustomParamRequestType =   new RequestType<ICommonParam, number, void>('custom/paramRequest');

export async function takeOneTest(
	client: LanguageClient,
    message: string
) {
    const result = await client.sendRequest(LinterStatusRequestType, {
        param: message
    });
}
```



## 服务端

之前定义的 `CustomParamRequestType`，Rust 端需要完全遵守，Rust TowerLSP 实现自定义消息的方法比较麻烦，请看仔细：

```rust
#[derive(Deserialize, Serialize, Debug)]
pub struct CustomParamRequestParams {
    param: String,
}

#[derive(Clone)]
pub struct CustomParamRequest;
// ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ 👇 这里定义了消息函数的输入，仔细看这个结构体和上文的 ICommonParam 完全一致
impl <'a>tower_lsp::jsonrpc::Method<&'a Arc<Backend>, (CustomParamRequestParams, ), Result<i32>> for CustomParamRequest {
    // 这里定义了消息函数的输出，和上文一致，是数字
    type Future = future::Ready<Result<i32>>;

    fn invoke(&self, _server: &'a Arc<Backend>, _params: (CustomParamRequestParams, )) -> Self::Future {
        future::ready(custom_param_request(_params.0.param))
    }
}

pub fn custom_param_request(param: String) -> Result<i32> {
    info!("receive param: {:?}", param);
    Ok(123)
}
```

定义完成的结构体需要在 TowerLSP 初始化的时候去进行注册：

```rust
#[tokio::main]
async fn main() {
    let _ = Opt::from_args();
    let log_handle = flexi_logger::Logger::with(flexi_logger::LogSpecification::info())
        .start()
        .unwrap();
    
    info!("launch Digital LSP, version: 0.4.0");

    let stdin = tokio::io::stdin();
    let stdout = tokio::io::stdout();

    let (service, socket) = LspService::build(|client| {
        let backend = Arc::new(Backend::new(client, log_handle));
        backend
    })
    	// 此处注册为 custom/paramRequest 和上文保持一致
        .custom_method("custom/paramRequest", CustomParamRequest)   // for test
        .finish();

    Server::new(stdin, stdout, socket)
        .serve(service)
        .await;
}
```