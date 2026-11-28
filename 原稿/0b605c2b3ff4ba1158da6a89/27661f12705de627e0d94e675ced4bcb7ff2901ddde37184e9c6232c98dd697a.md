## 前言

如果想要动态展示一个线路的数据流向，线段的【脉冲特效】可以说是最佳选择之一，今天就分享一个使用 svg 实现【脉冲特效】的例子。核心就是巧用 svg 内的各种方法，比如先在 `defs` 中定义好路线本身。然后再 g 中利用 `use` 关键词创建路径本身，再基于 js 中的 `KeyframeEffect` 对象基于同一条路径创建渐变特效。

---

## 代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
<svg viewBox="0 0 300 100" xmlns="http://www.w3.org/2000/svg">
    <defs>
        <radialGradient id="g1">
        <stop offset="0%" stop-color="white" />
        <stop offset="25%" stop-color="#CB81DA"" />
        <stop offset="95%" stop-opacity="0" stop-color="rgb(70, 70, 222)" />
        </radialGradient>
        <mask id="m1">
        <use id="u1" href="#p1" 
        stroke="white" stroke-width="5" fill="none"
        stroke-dasharray="100 200" stroke-linecap="round" />
        </mask>
        <path id="p1" d="M 0 0 q 68 -78 84 -59 Q 116 -24 129 -59
        Q 147 -113 162 -56 Q 174 -8 210 -60 Q 245 -111 300 -56 L 358 0" />
    </defs>
    <use href="#p1" transform="translate(0 100)" stroke="#CB81DA"
        stroke-width="1" fill="rgba(203, 129, 218, 0.1)"/>
    <g transform="translate(0 100)" mask="url(#m1)">
        <circle r="50" fill="url(#g1)">
        <animateMotion dur="5000ms" repeatCount="indefinite">
            <mpath href="#p1" />
        </animateMotion>
        </circle>
    </g>
</svg>
<script>
let p1 = document.getElementById('p1');
let u1 = document.getElementById('u1');

var u1Keyframes = new KeyframeEffect(
  u1, [{
      strokeDasharray: `100 ${p1.getTotalLength(p1)}`,
      strokeDashoffset: "100"
    },
    {
      strokeDasharray: `100 ${p1.getTotalLength(p1)}`,
      strokeDashoffset: `-${p1.getTotalLength(p1) - 100}`
    }
  ], {
    duration: 5000,
    iterations: Infinity
  }
);

var pulsationAnimation = new Animation(u1Keyframes, document.timeline);
pulsationAnimation.play();

// 播放动画 play()
// 暂停播放 pause()
// 重置状态 cancel()
</script>
</body>
</html>

<style>
html, body {
    background-color: #1e1e1e;
}
</style>
```

效果：

<iframe src="https://kirigaya.cn/files/html/pulsation" height=450 width="100%">
</iframe>

---

## 封装代码

对于上述代码，可以做出一些简单的封装，但是具体任务场景下，封装还是需要根据任务本身对于 path 数据的自由度来设计。此处只是给出一种简单的封装：

```js
let PluseIDCount = 0;

class PulseLine {
    constructor() {
        // 维护这部分的 id
        PluseIDCount++;
        this.pluseId = PluseIDCount;
    }

    loadData(pathString) {
        this.path = pathString
    }

    /**
     * @param {PulseLineSvgConfig} config 
     * @returns 
     */
    svgDoc(config) {
        config = config || {};
        const pluseStartColor = config.pluseStartColor || 'white';
        const pluseMiddleColor = config.pluseMiddleColor || '#CB81DA';
        const pluseEndColor = config.pluseEndColor || 'rgb(70, 70, 222)';
        const transform = config.transform || 'translate(0 0)';
        const duration = config.duration || 5000;

        this.duration = duration;

        const gId = 'g' + this.pluseId;
        const pId = 'p' + this.pluseId;
        const mId = 'm' + this.pluseId;
        const uId = 'u' + this.pluseId;
        const pathString = this.path;

        const parser = new DOMParser();

        const svgString = `<svg viewBox="0 0 300 100" xmlns="http://www.w3.org/2000/svg">
    <defs>
        <radialGradient id="${gId}">
        <stop offset="0%" stop-color="${pluseStartColor}" />
        <stop offset="25%" stop-color="${pluseMiddleColor}" />
        <stop offset="95%" stop-opacity="0" stop-color="${pluseEndColor}" />
        </radialGradient>
        <mask id="${mId}">
        <use id="${uId}" href="#${pId}" 
            stroke="white" stroke-width="5" fill="none"
            stroke-dasharray="100 200" stroke-linecap="round" />
        </mask>
        <path id="${pId}" d="${pathString}" />
    </defs>
    <use href="#${pId}" transform="${transform}" stroke="#CB81DA"
        stroke-width="1" fill="rgba(203, 129, 218, 0.1)"/>
    <g transform="${transform}" mask="url(#${mId})">
        <circle r="50" fill="url(#${gId})">
        <animateMotion dur="5000ms" repeatCount="indefinite">
            <mpath href="#${pId}" />
        </animateMotion>
        </circle>
    </g>
</svg>`;

        const svgDoc = parser.parseFromString(svgString, 'image/svg+xml');
        return svgDoc;
    }

    animation() {
        const pId = 'p' + this.pluseId;
        const uId = 'u' + this.pluseId;

        const pathElement = document.getElementById(pId);
        const pluseElement = document.getElementById(uId);

        const pathLength = pathElement.getTotalLength();

        const keyframes = new KeyframeEffect(
            pluseElement, [{
                strokeDasharray: `100 ${pathLength}`,
                strokeDashoffset: "100"
            },
            {
                strokeDasharray: `100 ${pathLength}`,
                strokeDashoffset: `-${pathLength - 100}`
            }
        ], {
            duration: this.duration,
            iterations: Infinity
        }
        );

        const pulsationAnimation = new Animation(keyframes, document.timeline);
        return pulsationAnimation;
    }
}

/**
 * @typedef PulseLineSvgConfig
 * @property {string} pluseStartColor
 * @property {string} pluseMiddleColor
 * @property {string} pluseEndColor
 * @property {string} transform
 * @property {number} duration
 */
```

使用起来非常方便：


```js
window.onload = () => {
    const pluseLine = new PulseLine();
    pluseLine.loadData('M 0 0 q 68 -78 84 -59 Q 116 -24 129 -59 Q 147 -113 162 -56 Q 174 -8 210 -60 Q 245 -111 300 -56 L 358 0');
    const svgDoc = pluseLine.svgDoc({
        pluseStartColor: 'white',
        pluseMiddleColor: '#CB81DA',
        pluseEndColor: 'rgb(70, 70, 222)',
        transform: 'translate(0 100)',
        duration: 5000
    });

    // 渲染 svg
    document.body.appendChild(svgDoc.documentElement);

    // 开启动画
    const animation = pluseLine.animation();
    animation.play();
}
```

上面其实有一个坑，那就是驱动 Animation 和 svg 中创建的 `animateMotion` 的时间帧不一定同步（而这个例子是在一个完全没有负载的 html 的初始化执行的，这导致这两个动画的动画帧几乎一致，但是如果是临时创建，那就未必了），所以需要进行同步：

```js
requestAnimationFrame(() => {
    const motion = g.selectAll('animateMotion').node();
    console.log(motion);
    motion.setAttribute('begin', '0s');
    motion.beginElement(); 
    pulsationAnimation.play();
});
```