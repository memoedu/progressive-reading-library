## 前言

之前运行一个几年前的 nodejs 项目时，发生了下面的报错：

```
[webpack-cli] Error: error:0308010C:digital envelope routines::unsupported
    at new Hash (node:internal/crypto/hash:67:19)
    at Object.createHash (node:crypto:130:10)
    at BulkUpdateDecorator.hashFactory (/opt/src/node_modules/webpack/lib/util/createHash.js:155:18)
    at BulkUpdateDecorator.digest (/opt/src/node_modules/webpack/lib/util/createHash.js:80:21)
    at /opt/src/node_modules/webpack/lib/DefinePlugin.js:595:38
    at Hook.eval [as call] (eval at create (/opt/src/node_modules/tapable/lib/HookCodeFactory.js:19:10), <anonymous>:100:1)
    at Hook.CALL_DELEGATE [as _call] (/opt/src/node_modules/tapable/lib/Hook.js:14:14)
    at Compiler.newCompilation (/opt/src/node_modules/webpack/lib/Compiler.js:1053:26)
    at /opt/src/node_modules/webpack/lib/Compiler.js:1097:29
    at Hook.eval [as callAsync] (eval at create (/opt/src/node_modules/tapable/lib/HookCodeFactory.js:33:10), <anonymous>:6:1) {
  opensslErrorStack: [ 'error:03000086:digital envelope routines::initialization error' ],
  library: 'digital envelope routines',
  reason: 'unsupported',
  code: 'ERR_OSSL_EVP_UNSUPPORTED'
}
```

如果是运行 `npm run build` ，也会发生类似的错误：

```
js/chunk-vendors.f973b960.js from Terser
Error: error:0308010C:digital envelope routines::unsupported
    at new Hash (node:internal/crypto/hash:69:19)
	...
```

总结关键词：

- digital envelope routines::unsupported
- ERR_OSSL_EVP_UNSUPPORTED

## 原因

简单搜了一下，发现是因为 webpack 版本的问题，如果你版本大于等于 17 的 nodejs 上运行了老版本的 webpack，就会出现上面的错误，这是因为新版本 nodejs 用的 openssl 程序集和老版本的 webpack 的不兼容了，从而导致 webpack 拒绝工作。



## 解决方法

解决方法就是通过环境变量，告知 nodejs 使用旧版本的 openssl 算法集

```bash
# Windows powershell
$Env:NODE_OPTIONS="--openssl-legacy-provider"

# Linux / MacOS
export NODE_OPTIONS=--openssl-legacy-provider

# Windows cmd
set NODE_OPTIONS=--openssl-legacy-provider
```





