---
title: "Clipboard"
date: 2023-02-23 13:55:30 +08:00
draft: false
categories: [JavaScript]
tags: []
card: false
weight: 0
---

向剪切板写入 图片等任意的数据到剪贴板。 这个方法可以用于实现剪切和复制的功能,是异步的

```js
document.body.addEventListener(
  'click',
  async (e) => {
    await navigator.clipboard.writeText('Yo')
  }
)
```

Clipboard.read()用于读取剪切板的数据，也是异步的成功返回数据

```js
async function getClipboardContents() {
  try {
    const text = await navigator.clipboard.readText();
    console.log('Pasted content: ', text);
  } catch (err) {
    console.error('Failed to read clipboard contents: ', err);
  }
}
```

