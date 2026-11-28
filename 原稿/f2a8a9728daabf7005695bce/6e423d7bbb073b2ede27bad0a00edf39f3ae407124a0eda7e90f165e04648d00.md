## 前言

最近在自己的项目里遇到了数据持久化的需求，需要转化 ArrayBuffer 这个烦人的对象。这篇文章作为一个随笔，我想起来就会来记录和 `ArrayBuffer`, `Blob` 还有 base64 相关的技术。

---

## ArrayBuffer 和 base64 相互转换

因为网络最常用的数据载体 json 中的基本数据结构是 number 和 string，所以，我们遇到需要传输 ArrayBuffer，一个低效但是简单的方法就是将它转换为 string，而 base64 编码就是做转换的一个比较简单的方法。

```typescript
function arrayBufferToBase64(buffer: ArrayBuffer): string {
    let binary = '';
    const bytes = new Uint8Array(buffer);
    const len = bytes.byteLength;
    for (let i = 0; i < len; i++) {
        binary += String.fromCharCode(bytes[i]);
    }
    return window.btoa(binary);
}

function base64ToArrayBuffer(base64: string): ArrayBuffer {
    const binary_string = window.atob(base64);
    const len = binary_string.length;
    const bytes = new Uint8Array(len);
    for (let i = 0; i < len; i++) {
        bytes[i] = binary_string.charCodeAt(i);
    }
    return bytes.buffer;
}
```

---

## ArrayBuffer 转换为 Blob

很多时候，我们希望可以将 ArrayBuffer 数据（不管你是怎么拿到的），注入一些原生 HTML 对象，比如 Image 上，这个时候就需要使用 Blob。因为大部分浏览器上的默认对象的网络载入都是 js 干涉不到的，比如图像，css的网络请求，js 端只能发起，但是做不到拦截等更加复杂的处理。因此，如果你需要将 ArrayBuffer 注入比如 Image 这样的对象中，需要通过 Blob 将 ArrayBuffer 变成一个网页自己发起的网页链接：

```typescript
const img = new Image();
img.onload = () => {
    // 图像数据载入后的行为
    ...
}

// 确定图像的后缀，从而为 blob 构建 MIME type
const imageFormat = imageUrl.split('.').at(-1).toLowerCase();

const response = fetch(imageUrl);
const arraybuffer = await response.arrayBuffer();
const blob = new Blob([arraybuffer], { type: 'image/' + imageFormat });
const url = URL.createObjectURL(blob);
img.src = url;
```


