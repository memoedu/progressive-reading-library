最近算是给一个不小的wasm编译项目的1.x版本收个尾，正好用到了mocha框架对好兄弟写的C++编译的wasm进行一键测试：

<div align=center>
<img src="https://pic3.zhimg.com/v2-e90daab689e803c697fb32afa369eeda_r.jpg" style="width: 60%;"/>
<p class="img-caption">wasm项目测试</p>
</div>

笔者之前其实就已经大规模使用过这个框架了。这篇文章，就简单总结一下这5个月的实践中，mocha框架的简易使用过程。

---

## Why mocha？

UT（单元测试）是大规模软件开发中不可缺少的一个模块。比如你花了一下午写了几千行代码后，肯定有点心慌，因为你不知道你的代码是否符合要求，特别是JavaScript这种容易写成屎山的编程语言。

还有，下次谁把typescript写成anyscript别给我逮到。

<div align=center>
<img src="https://pic3.zhimg.com/v2-9ff4f15453e12ebfbfd06422fbf4d942_b.gif" style="width: 60%;"/>
<p class="img-caption">不要写anyscript</p>
</div>

这个时候，就可以跑一跑测试工程师或者项目管理者写好的单元测试，如果测试样例全过了，那么你大概率今天就可以开始和好朋友开黑了。这种开发模型，也被我们称为测试驱动开发（TDD），如果你在维护自己的开源大型项目，TDD是一种非常利用项目CI的模式。比如知名的开源项目vscode，它的CI模式就是团队贴出这个版本需要实现的函数，contributor选择自己可以实现的函数，实现后，运行官方的UT，全部通过后，就可以提交PR了！

而mocha是一款很成熟的，专为JavaScript设计的UT框架，可以很好满足大部分情况下编写UT的需求。下面就简单讲一下如何使用mocha。

---

## 安装

首先，我们简单进入一个用于演示的文件夹mochaLearn，并安装mocha：

```bash
cd mochaLearn
npm init
npm install mocha
```

## 设置测试脚本
进入package.json，在scripts修改或加入：
```json
  "scripts": {
    "test": "mocha"
  },
```

## 编写功能函数
新建一个./function.js，写入如下内容：

```javascript
/**
 * @param {number} a 
 * @param {number} b 
 * @returns {number}
 */
function simple_add(a, b) {
    return a + b;
}

/**
 * @param {number} n 
 * @returns {number}
 */
function is_prime(n) {
    // 故意写错
    if (n % 2 == 0) {
        return false;
    }

    for (let i = 3; i < parseInt(Math.sqrt(n)); i += 2) {
        if (n % i == 0) {
            return false
        }
    }

    return true;
}

module.exports = {
    is_prime,
    simple_add
}
```
下面我们就来测试上面两个函数。

## mocha测试入口

mocha需要一个入口js文件用于确定测试的文件和测试的属性等诸多属性。我们创建./test/index.js写入如下内容：

```javascript
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

            files.forEach(f => mocha.addFile(path.resolve(testsRoot, f)));

            try {

                // 设置每个测试的超时
                mocha.timeout(60000);
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

run();
```

这个文件意味着，mocha会自动测试./tests文件夹下所有的以.test.js结尾的文件：


在vscode中，如果你安装了material icon，.test.js的文件也有专属的图标
一般而言，一个JS文件或者一个CJS/ES6模块对应一个测试文件。于是，我们创建./test/suite/function.test.js负责测试./function.js中导出的函数。

## 测试用例

编写如下的测试样例：

```javascript
const { simple_add, is_prime } = require('../../function');
const assert = require('assert');

suite('test function.js', () => {
    // 可以这样直接测试
    test('simple_add', () => {
        assert.equal(simple_add(1, 2), 3);
    });

    // 也可以事先写好测试集，写在测试程序里或者从静态文件中读入
    const is_prime_suites = [
        { input: 2, expect: true },
        { input: 3, expect: true },
        { input: 10, expect: false },
        { input: 23, expect: true },
        { input: 30, expect: false },
    ];

    for (const s of is_prime_suites) {
        test(`is_prime(${s.input}) => ${s.expect}`, () => {
            assert.equal(is_prime(s.input), s.expect);
        });
    }
});
```

运行npm run test运行测试，得到如下的结果：

```bash
> mochalearn@1.0.0 test
> mocha

  test function.js
    ✔ simple_add
    1) is_prime(2) => true
    ✔ is_prime(3) => true
    ✔ is_prime(10) => false
    ✔ is_prime(23) => true
    ✔ is_prime(30) => false


  5 passing (4ms)
  1 failing

  1) test function.js
       is_prime(2) => true:

      AssertionError [ERR_ASSERTION]: false == true
      + expected - actual

      -false
      +true

      at Context.<anonymous> (test\suite\function.test.js:21:20)
      at process.processImmediate (node:internal/timers:471:21)

```
发现2这个样例没有通过，检查一下原函数，发现忘记判断2了，遂修改。

---

## 总结
mocha还是有许多的功能的，不过我上面给出的case足够应付许多情况了，更多看各位如果多快好省地编写合理，高覆盖的测试样例了。