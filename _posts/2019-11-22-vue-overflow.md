---
layout: post
title: 'Vue自定义指令--实现文字溢出显示...鼠标移入浮层展示全部'
subtitle: '整理一下敲一敲常用的js方法'
date: 2019-11-22
tags: Vue JavaScript
catalog: true
---

Vue项目中经常会遇到超出显示...鼠标移入弹框显示全部的需求

索性写一个自定义指令吧

不用关心别的，给需要超出隐藏显示全部的元素加一个自定义指令就完事🌶 

---
## 首先
* 要知道当前元素的宽
* 将文字放到一个容器中，将容器的样式（主要是有关字体的样式）都设置为当前元素的样式，然后获取容器的宽，也就是文字的宽
* 如果文字的宽度超过了当前元素的宽度，则给溢出隐藏的css样式 `overflow :hidden;text-overflow: ellipsis;white-space: normal`
* 定义鼠标移入展示浮层，浮层中显示全部内容，鼠标移出销毁浮层

## 代码

* mian.js 中定义全局指令
* 也可以在组件的`directives `中注册局部指令

```js
Vue.directive('showTips', {
  // el {element} 当前元素
  componentUpdated (el) {
    const curStyle = window.getComputedStyle(el, '') // 获取当前元素的style
    const textSpan = document.createElement('span') // 创建一个容器来记录文字的width
    // 设置新容器的字体样式，确保与当前需要隐藏的样式相同
    textSpan.style.fontSize = curStyle.fontSize
    textSpan.style.fontWeight = curStyle.fontWeight
    textSpan.style.fontFamily = curStyle.fontFamily
    // 将容器插入body，如果不插入，offsetWidth为0
    document.body.appendChild(textSpan)
    // 设置新容器的文字
    textSpan.innerHTML = el.innerText
    // 如果字体元素大于当前元素，则需要隐藏
    if (textSpan.offsetWidth > el.offsetWidth) {
      // 给当前元素设置超出隐藏
      el.style.overflow = 'hidden'
      el.style.textOverflow = 'ellipsis'
      el.style.whiteSpace = 'nowrap'
      // 鼠标移入
      el.onmouseenter = function (e) {
        // 创建浮层元素并设置样式
        const vcTooltipDom = document.createElement('div')
        vcTooltipDom.style.cssText = `
          max-width:400px;
          max-height: 400px;
          overflow: auto;
          position:absolute;
          top:${e.clientY + 5}px;
          left:${e.clientX}px;
          background: rgba(0, 0 , 0, .6);
          color:#fff;
          border-radius:5px;
          padding:10px;
          display:inline-block;
          font-size:12px;
          z-index:19999
        `
        // 设置id方便寻找
        vcTooltipDom.setAttribute('id', 'vc-tooltip')
        // 将浮层插入到body中
        document.body.appendChild(vcTooltipDom)
        // 浮层中的文字
        document.getElementById('vc-tooltip').innerHTML = el.innerText
      }
      // 鼠标移出
      el.onmouseleave = function () {
        // 找到浮层元素并移出
        const vcTooltipDom = document.getElementById('vc-tooltip')
        vcTooltipDom && document.body.removeChild(vcTooltipDom)
      }
    }
    // 记得移除刚刚创建的记录文字的容器
    document.body.removeChild(textSpan)
  },
  // 指令与元素解绑时
  unbind () {
    // 找到浮层元素并移除
    const vcTooltipDom = document.getElementById('vc-tooltip')
    vcTooltipDom && document.body.removeChild(vcTooltipDom)
  }
})
```

* 使用 : 需要溢出隐藏的直接加上指令 `v-show-tips` 即可
```html
<div v-show-tips class="title-text">{{ name }}</div>
```
