之前有一次因为配置文件丢失的原因，我yo code了一个新的项目，再去扒它的配置文件，好生狼狈。。。故于此记录我的vscode插件开发基本配置，免得找不到。

## .gitignore

yo code貌似不会自动创建`.gitignore`，请创建并至少写入：
```
node_modules
```

我不希望看到以后的项目中有哪个小可爱再把 node_modules push到remote仓库。

## launch.json

如果按下F5没有反应，那一定是你的`.vscode/launch.json`无了，记得补。一个包含了基本的扩展运行和测试运行的测试如下：

```json
{
	"version": "0.2.0",
	"configurations": [
		{
			"name": "Extension Run",
			"type": "extensionHost",
			"request": "launch",
			"runtimeExecutable": "${execPath}",
			"args": [
				"--extensionDevelopmentPath=${workspaceFolder}"
			]
		},
		{
			"name": "Extension Tests",
			"type": "extensionHost",
			"request": "launch",
			"args": [
				"--extensionDevelopmentPath=${workspaceFolder}",
				"--extensionTestsPath=${workspaceFolder}/test/suite/index"
			]
		}
    ],
    "resolveSourceMapLocations": [
        "${workspaceFolder}/**",
        "!**/node_modules/**"
    ]
}
```

其中进行测试的时候，测试的输出会在控制台的“输出”一栏展示，会有一个扩展窗口打开，请不要在意，因为这个临时打开的扩展窗口的唯一作用就是为测试提供必要的某些vscode扩展的标注库的运行时，比如vscode。换句话说，如果没有这个窗口，那么我们require("vscode")是无法找到对应的库的，这也就是为什么我们需要在Extension Tests的args下加一个"--extensionDevelopmentPath=${workspaceFolder}"的原因了。

## test suite
测试集使用mocha，在yo code的时候，这个为js单元测试设计的库就应该加入了package.json中，请检查，养成为测试驱动的开发习惯将受益终生。

在上面的配置中，Extension Tests的入口指向了${workspaceFolder}/test/suite/index，那么就创建${workspaceFolder}/test/suite/index.js并写入如下内容：
```js
const path = require('path');
const Mocha = require('mocha');
const glob = require('glob');

function run() {
	// Create the mocha test
	const mocha = new Mocha({
		ui: 'tdd',
		color: true
	});

	const testsRoot = path.resolve(__dirname, '..');

	return new Promise((c, e) => {
		glob('**/**.test.js', { cwd: testsRoot }, (err, files) => {
			if (err) {
				return e(err);
			}

			// Add files to the test suite
			files.forEach(f => mocha.addFile(path.resolve(testsRoot, f)));

			try {
				// Run the mocha test
				mocha.run(failures => {
					if (failures > 0) {
						e(new Error(`${failures} tests failed.`));
					} else {
						c();
					}
				});
			} catch (err) {
				console.error(err);
				e(err);
			}
		});
	});
}

module.exports = {
	run
};
```

后续的测试文件都写在`index.js`同级目录下，且以`.test.js`作为后缀，测试程序会自动去index.js的同级目录下捕获所有的文件，并找到其中的`**.test.js`进行执行。文件深度可以任意，可以在index.js同级目录下创建测试文件，也可以在同级目录下创建文件夹，再创建。