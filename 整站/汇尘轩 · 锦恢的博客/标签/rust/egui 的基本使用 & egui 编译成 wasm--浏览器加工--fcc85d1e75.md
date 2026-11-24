## 前言

egui 是一项好技术，它可以只用一份 rust 代码就编译到 web 和 native 上运行。它在未来会渗透进入不少的领域。这篇博客作为我探索 egui 的开山鼻祖，我打算简单探索一下目前 egui 的能做到什么程度，是否足够稳定。

先来看看如何配置基础的环境吧！

---

## 搭建项目

首先创建一个新的项目 `learn_egui`

```bash
cargo new learn_egui && cd learn_egui
```

egui 的版本经历过一次重大更新，`0.3.x` 和 `0.2.x` 的 API 是互相不兼容的。此处强烈推荐使用最新的 `0.3.0` 版本，下面是本地使用到的第三方库（egui 的版本是非常重要的一件事，前往不能选择错）

文件名：`Cargo.toml`

```toml
[package]
name = "learn_egui"
version = "0.1.0"
edition = "2021"

[dependencies]
egui = "0.30.0"
eframe = { version = "0.30", default-features = false, features = [
    "accesskit",     # Make egui compatible with screen readers. NOTE: adds a lot of dependencies.
    "default_fonts", # Embed the default egui fonts.
    "glow",          # Use the glow rendering backend. Alternative: "wgpu".
    "persistence",   # Enable restoring app state when restarting the app.
    "wayland",       # To support Linux (and CI)
] }
log = "0.4"
serde = { version = "1", features = ["derive"] }
wasm-bindgen = "0.2"

# native
[target.'cfg(not(target_arch = "wasm32"))'.dependencies]
env_logger = "0.11"

# web
[target.'cfg(target_arch = "wasm32")'.dependencies]
wasm-bindgen-futures = "0.4"
web-sys = "0.3.70"

[profile.release]
opt-level = 2       # o2 优化，生成更快体积更小的 wasm
```

---

## hello egui

首先，我们搭建一个最简单的 egui 的例程，后续所有的代码都可以在这个模板上进行增添或者修改。

### 项目结构

首先，创建如下的文件结构：

```
 📦learn_egui
  ┣ 📂src
  ┃ ┣ 📜app.rs
  ┃ ┣ 📜lib.rs
  ┃ ┗ 📜main.rs
  ┣ 📂web
  ┃ ┗ 📜index.html
  ┣ 📜.gitignore
  ┣ 📜Cargo.lock
  ┗ 📜Cargo.toml
```

UI 相关的代码我们都写在 `app.rs` 中，`main.rs` 中负责处理不同平台中 egui 要如何启动。

### 第一个简单的原生应用

虽说我们的目的是使用 egui 写 web，但是在那之前，我们需要先通过一个 hello world 例子来熟悉一下 egui 的基本例子。

我们现在 egui 的画布上画点基础形状和基本文字吧！

文件名：`app.rs`

```rust
use eframe::egui;
use wasm_bindgen::prelude::*;

#[wasm_bindgen]
pub struct MyApp {
    counter: i32
}

impl eframe::App for MyApp {
    fn update(&mut self, ctx: &egui::Context, _frame: &mut eframe::Frame) {
        egui::CentralPanel::default().show(ctx, |ui| {
            // 获取 Painter
            let painter = ui.painter();

            // 绘制一个矩形
            painter.rect(
                egui::Rect::from_min_size(egui::Pos2::new(50.0, 50.0), egui::Vec2::new(200.0, 100.0)),
                5.0, // 圆角半径
                egui::Color32::from_rgb(100, 200, 100), // 填充颜色
                egui::Stroke::new(2.0, egui::Color32::from_rgb(255, 0, 0)), // 边框
            );

            // 绘制一个圆形
            painter.circle(
                egui::Pos2::new(150.0, 150.0),
                50.0, // 半径
                egui::Color32::from_rgb(200, 100, 100), // 填充颜色
                egui::Stroke::new(2.0, egui::Color32::from_rgb(0, 0, 255)), // 边框
            );

            // 绘制一条线
            painter.line_segment(
                [egui::Pos2::new(50.0, 200.0), egui::Pos2::new(250.0, 200.0)],
                egui::Stroke::new(3.0, egui::Color32::from_rgb(0, 255, 0)), // 线条颜色
            );

            // 绘制文本
            painter.text(
                egui::Pos2::new(100.0, 250.0),
                egui::Align2::CENTER_CENTER,
                "Hello, egui!",
                egui::FontId::proportional(24.0),
                egui::Color32::from_rgb(255, 255, 255), // 文本颜色
            );

            // 处理鼠标点击事件
            ui.input(|input| {
                if input.pointer.primary_clicked() {
                    println!("Mouse clicked at: {:?}", input.pointer.interact_pos());
                }
            });
        });
    }

}
```

egui 作为即时渲染框架，它的每一帧都是重新绘制的，在函数 `update` 中定义了每一帧要如何进行绘制，对于需要高性能的场景，这么做非常可以获取比一般的框架更加高效的性能。 `egui::CentralPanel` 则是 egui 的中央面板，也可以理解为是一个主面板，对于布局比较简单的项目，一个中央面板就能装下你需要绘制的内容了。	

在 `lib.rs` 中定义一下项目的模块树。

文件名：`lib.rs`

```rust
mod app;
pub use app::*;
```

最后在 `main.rs` 中定义一下原生平台的入口函数。

文件名：`main.rs`

```rust
use eframe::*;
use learn_egui::*;

#[cfg(not(target_arch = "wasm32"))]
fn main() -> eframe::Result {
    let options = eframe::NativeOptions {
        viewport: egui::ViewportBuilder::default().with_inner_size([600.0, 500.0]),
        ..Default::default()
    };
    
    eframe::run_native(
        "My egui App",
        options,
        Box::new(|_| {
            Ok(Box::new(MyApp::new()))
        })
    )
}
```

运行一下看看结果：

![](https://picx.zhimg.com/80/v2-f170bf3efece2c2812bb311ab040036f_720w.png)

可以看到，渲染得很漂亮。

### 编译到 web 上去！

让我们更进一步吧，接下来才是重头戏，egui 的代码不仅可以编译成桌面的原生应用，也就是我们常说的 native，它也可以编译成 wasm，放到 web 上去运行。eframe 这个库非常强大地完成了不同平台上渲染机制的翻译和适配，native 上我们的代码是通过本地的 directX，opengl 或者 vulkun 后端渲染的，那么 web 上就是通过 webgl，webgl2 或者 wgpu 完成的渲染，eframe 的工程师已经完成了内部各类复杂算法的实现，开发者不再需要去关心底层的渲染细节。只需要关心顶层的业务逻辑，感恩！

想要把上面的代码编译成 wasm，首先需要安装基本的工具链和生成 js 胶水文件的 bindgen 命令行工具：

```bash
rustup target add wasm32-unknown-unknown
cargo install wasm-bindgen-cli
```

这个操作会消耗不少时间，阁下可以去泡杯咖啡。

然后我们需要定义 web 环境下，我们的应用程序要如何被初始化，在 `main.rs` 中追加下面的代码：

```rust
#[cfg(target_arch = "wasm32")]
fn main() {
    use eframe::wasm_bindgen::JsCast as _;

    // 将 log 的信息重定向到 js 的 console.log
    eframe::WebLogger::init(log::LevelFilter::Debug).ok();
    let web_options = eframe::WebOptions::default();

    wasm_bindgen_futures::spawn_local(async {
        let document = web_sys::window()
            .expect("No window")
            .document()
            .expect("No document");

        // 在 id 为 canvas 的元素上创建 egui 的画面
        let canvas = document
            .get_element_by_id("canvas")
            .expect("Failed to find canvas")
            .dyn_into::<web_sys::HtmlCanvasElement>()
            .expect("the_canvas_id was not a HtmlCanvasElement");

        // 启动
        let start_result = eframe::WebRunner::new()
            .start(
                canvas,
                web_options,
                Box::new(|cc| {
                    Ok(Box::new(MyApp::new()))
                }),
            )
            .await;
    });
}
```

然后编译到 wasm：

```bash
cargo build --target wasm32-unknown-unknown --release
```

这会在 `target/wasm32-unknown-unknown/release` 下生成 `learn_egui.wasm`，当然只有 wasm 是不够的，毕竟它不是 wasi，在过往我们一般会通过 emcc 工具链生成对应的胶水，在这里，我们可以利用 wasm-bindgen 自动根据项目创建我们运行 wasm 需要的胶水，wasm-bindgen 还会利用 wasm 项目中的 proc-macro 来自动创建 `.d.ts` ，再依靠最先进的 LSP 系统，可以大幅度降低跨语言开发的成本消耗。伟大，无需多言。

```bash
wasm-bindgen .\target\wasm32-unknown-unknown\release\learn_egui.wasm --out-dir ./web --target web
```

这运行完成后，会在 `web` 中生成 wasm（其实是搬运的）和 js 胶水还有用于自动补全的 `.d.ts` 。

我们在 `web/index.html` 中完成 wasm 的导入：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">

</head>
<body>
	<title>Document</title>
    <canvas id="canvas"></canvas>
    <script type="module">
    import init from './learn_egui.js';
    async function run() {
        await init();
    }
    run();
    </script>
</body>
</html>

<style>
html {
    background-color: #1b1b1b;
}
#canvas {
    height: 400px;
    width: 600px;
}
</style>
```

然后运行 html，效果如下：

![](https://picx.zhimg.com/80/v2-4c67cb4c5228a3f0251575fcd158aa1b_720w.png)

---

## 更加优雅地开发 wasm

刚刚的开发过程有两个显著缺点：

1. 开发过程中 `#[cfg(target_arch = "wasm32")]` 让下方的代码无法被 lsp 识别到，从而无法自动补全。
2. 生成的 wasm 中，我们无法在启动后与 js 的运行时获取到 app 句柄，从而在 js 层面无法去获取内部的数据或者操作内部函数，比如 egui 的重新渲染。

下面我们来解决这两个问题。

### 解决 cfg 宏下方代码无法识别的问题

为了更加惬意地开发代码，创建 `.vscode/settings.json`：

```json
{
    "rust-analyzer.cargo.target": "wasm32-unknown-unknown"
}
```

重启 vscode，此时 `#[cfg(target_arch = "wasm32")]` 下的代码被激活，自动补全生效。

### 通过 WebHandle 在 js 中创建 app 句柄

除了通过 `WebRunner` 的 API 外，egui 还提供了一种方式，允许用户在 js 层面来手动创建 app，这样用户自己就能拿到 egui 的 app 句柄了，从而我们就可以从 js 层面去干预 egui 内部的执行了。

文件名：`lib.rs`

```rust
mod app;
pub use app::*;

#[cfg(target_arch = "wasm32")]
mod web;

#[cfg(target_arch = "wasm32")]
pub use web::*;
```

在 `lib.rs` 同级创建 `web.rs`，内容如下：

```rust
use wasm_bindgen::prelude::*;

use crate::MyApp;

#[derive(Clone)]
#[wasm_bindgen]
pub struct WebHandle {
    runner: eframe::WebRunner,
}


#[wasm_bindgen]
impl WebHandle {
    #[wasm_bindgen(constructor)]
    pub fn new() -> Self {
        // 将 log 的信息重定向到 js 的 console.log
        let log_level = if cfg!(debug_assertions) {
            log::LevelFilter::Trace
        } else {
            log::LevelFilter::Debug
        };
        eframe::WebLogger::init(log_level).ok();

        Self {
            runner: eframe::WebRunner::new(),
        }
    }

    #[wasm_bindgen]
    pub async fn start(
        &self,
        canvas: web_sys::HtmlCanvasElement,
    ) -> Result<(), wasm_bindgen::JsValue> {
        self.runner
            .start(
                canvas,
                eframe::WebOptions::default(),
                Box::new(|cc: &eframe::CreationContext<'_>| {
                    Ok(Box::new(MyApp::new(cc)))
                }),
            )
            .await
    }

    #[wasm_bindgen]
    pub fn destroy(&self) {
        self.runner.destroy();
    }

    #[wasm_bindgen]
    pub fn increment(&self) {
        if let Some(mut app) = self.runner.app_mut::<MyApp>() {
            app.increment();
            app.context.request_repaint();
        }
    }

    #[wasm_bindgen]
    pub fn has_panicked(&self) -> bool {
        self.runner.has_panicked()
    }

    #[wasm_bindgen]
    pub fn panic_message(&self) -> Option<String> {
        self.runner.panic_summary().map(|s| s.message())
    }

    #[wasm_bindgen]
    pub fn panic_callstack(&self) -> Option<String> {
        self.runner.panic_summary().map(|s| s.callstack())
    }
}
```

稍微修改一下 `app.rs` 中的 MyApp 结构体，增加一些属性和方法，再给渲染的代码增加几个按钮。

文件名：`app.rs`

```rust
use std::sync::Arc;
use eframe::{egui, CreationContext};

pub struct MyApp {
    pub counter: i32,
    pub context: Arc<egui::Context>
}

// 用于 wasm 的外部函数
impl MyApp {
    pub fn new(cc: &CreationContext<'_>) -> Self {
        let context = cc.egui_ctx.clone();

        Self {
            counter: 0,
            context: Arc::new(context) 
        }
    }

    pub fn increment(&mut self) {
        self.counter += 1;
    }

    pub fn get_counter(&self) -> i32 {
        self.counter
    }
}

impl eframe::App for MyApp {
    #[cfg(target_arch = "wasm32")]
    fn as_any_mut(&mut self) -> Option<&mut dyn std::any::Any> {
        Some(&mut *self)
    }

    fn update(&mut self, ctx: &egui::Context, _frame: &mut eframe::Frame) {
        egui::CentralPanel::default().show(ctx, |ui| {
            // 获取 Painter
            let painter = ui.painter();

            ctx.request_repaint();

            // 绘制一个矩形
            painter.rect(
                egui::Rect::from_min_size(egui::Pos2::new(50.0, 50.0), egui::Vec2::new(200.0, 100.0)),
                5.0, // 圆角半径
                egui::Color32::from_rgb(100, 200, 100), // 填充颜色
                egui::Stroke::new(2.0, egui::Color32::from_rgb(255, 0, 0)), // 边框
            );

            // 绘制一个圆形
            painter.circle(
                egui::Pos2::new(150.0, 150.0),
                50.0, // 半径
                egui::Color32::from_rgb(200, 100, 100), // 填充颜色
                egui::Stroke::new(2.0, egui::Color32::from_rgb(0, 0, 255)), // 边框
            );

            // 绘制一条线
            painter.line_segment(
                [egui::Pos2::new(50.0, 200.0), egui::Pos2::new(250.0, 200.0)],
                egui::Stroke::new(3.0, egui::Color32::from_rgb(0, 255, 0)), // 线条颜色
            );

            // 绘制文本
            painter.text(
                egui::Pos2::new(100.0, 250.0),
                egui::Align2::CENTER_CENTER,
                "Hello, egui!",
                egui::FontId::proportional(24.0),
                egui::Color32::from_rgb(255, 255, 255), // 文本颜色
            );

            // 处理鼠标点击事件
            ui.input(|input| {
                if input.pointer.primary_clicked() {
                    println!("Mouse clicked at: {:?}", input.pointer.interact_pos());
                }
            });

            // 显示一个计数器
            ui.label(format!("Counter: {}", self.counter));
            
            // 创建按钮，并定义接受到它的讯息
            if ui.button("Increment").clicked() {
                self.counter += 1;
            }

            if ui.button("Reset").clicked() {
                self.counter = 0;
            }
        });
    }

}
```

编译一把：

```bash
cargo build --target wasm32-unknown-unknown --release
wasm-bindgen .\target\wasm32-unknown-unknown\release\learn_egui.wasm --out-dir ./web --target web
```

再修改一下 html，来手动创建 egui 的画布：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <canvas id="canvas"></canvas>
    <button id="button">increment from js</button>
<script type="module">
import init, { WebHandle } from './learn_egui.js';
let app;
async function launch() {
    await init();

    app = new WebHandle();
    const canvas = document.getElementById('canvas');
    await app.start(canvas);
} 

const button = document.getElementById('button');
button.addEventListener('click', () => {
    app.increment();
});

launch();
</script>
</body>
</html>

<style>
html {
    background-color: #1b1b1b;
}
#canvas {
    height: 400px;
    width: 600px;
}
#button {
    position: fixed;
    left: 15px;
    top: 100px;
    background-color: #3c3c3c;
    border-radius: .3em;
    color: #ababab;
    border: #3c3c3c solid 1px;
    user-select: none;
}

#button:hover {
    border: #d9d9d9 solid 1px;
    color: #d9d9d9;
}
</style>
```

在此，我们创建了一个外部的按钮和一个 rust 中就写好的内部按钮，这两个按钮都可以修改计数器，也就是说，我们可以在 js 层面去修改 egui 内部的数据结构了。

![](https://picx.zhimg.com/80/v2-adacfe96135ba9f9cf7e0d3f391f260d_720w.png)

点击上图的 `Increment` 或者 `increment from js` 都可以使得计数器增加。前者是在 rust 中实现的，后者是在 JavaScript 中实现的。

---

## 参考资料

- https://github.com/emilk/egui/blob/master/crates/egui_demo_app/src/web.rs
- https://github.com/emilk/egui/blob/d97cd8233858be557025d86ec5b2d59bc62ba669/web_demo/multiple_apps.html
- https://github.com/TheDestroyer19/timekeeper/blob/master/src/app.rs#L118