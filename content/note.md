---
title: Notes
---

开发 H5 时，设计走查后总是会提出边框太粗了问题。细究原因的话，设计师一般在 750
像素宽的设计稿里绘制 1 像素宽的边框，H5 的视口宽为 375 像素，所以最终的边框应
该设置成 0.5 像素。但由于移动端 H5 适配时，采取了 rem 的解决方案，导致在不同机
型中的结果不同，有可能是 0.52 像素，有可能是 0.48 像素，而这些像素值表现又不一
，浏览器渲染边框可能成功/可能失败。解决方案的话，最常见的就是通过 CSS 里的
scale 属性。

```css
.border::after {
  content: '';
  position: absolute;
  left: 0;
  top: 0;
  width: 200%;
  height: 200%;
  border: 1px solid black;
  transform: scale(0.5);
  /* transform: left left */
  transform-origin: 0 0;
}
```

100vw 等于视口的宽度。

移动设备中存在一个常见问题，即使使用 100vh，也会滚动，原因是地址栏的高度可见
  。Louis Hoebregts 写了一篇关于这个问题的文章，并给出了一个简单的解决方案。

```css
.my-element {
  height: 100vh; /* 不支持自定义属性的浏览器的回退 */
  height: calc(var(--vh, 1vh) * 100);
}
```

```js
// 首先，我们得到视口高度，我们乘以 1% 得到一个vh单位的值
let vh = window.innerHeight * 0.01
// 然后，将`--vh`自定义属性中的值设置为文档的根目录一个属性
document.documentElement.style.setProperty('--vh', `${vh}px`)
```

3 X 3布局使用flex：
```css
.container {
  display: flex;
  flex-wrap: wrap;
  --gap: 10px;
}
.item {
  height: 100px;
  flex-basis: calc((100% - 2 * var(--gap)) / 3);
  margin: 0 var(--gap) var(--gap) 0;
  background-color: black;
}
.item:nth-child(3n) {
  margin-right: 0;
}
```

懒加载
```js
const imgs = [...document.querySelectorAll('img')];
const load = (entries) => {
  entries.forEach(entry => {
    const { target, isIntersecting } = entry
    if (isIntersecting) {
      target.setAttribute('src', target.dataset.src);
      target.removeAttribute('data-src');
      observer.unobserve(target);
    }
  })
}
const observer = new IntersectionObserver(load);
imgs.forEach(img => observer.observe(img))
```
