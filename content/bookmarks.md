---
title: 'bookmarks'
---

<script>
  const header = document.querySelector('header')
  header.parentNode.removeChild(header)

  document.querySelector('h1').parentNode.classList.add('mt-12')

  setTimeout(() => {
    const nav = document.querySelector('footer nav');
    nav.parentNode.removeChild(nav);
  }, 0);
</script>

- [useEffect 完整指南](https://overreacted.io/zh-hans/a-complete-guide-to-useeffect/)

- [深入浅出 Viewport 设计原理](https://www.cnblogs.com/onepixel/p/12144364.html)

- [最后一次探究 1px](https://jelly.jd.com/article/5f5a4b31da524a0147e97da0#)

- [When to useLayoutEffect Instead of useEffect (example)](https://daveceddia.com/useeffect-vs-uselayouteffect/)