---
title: react
---

### useEffect 的秩序

函数组件通过 setState 触发了更新，可以理解为函数组件被重新调用了。函数组件调用会
返回新的 JSX，最终被渲染到浏览器的页面上。然后，上一次渲染周期里的 cleanup 函数
被调用，执行一些清理逻辑。再然后，才会执行 useEffect。

```jsx
const App = () => {
  const [count, setCount] = useState(0)

  useEffect(() => {
    console.log(`useEffect: ${count}`)
    return () => {
      console.log(`useEffect cleanup: ${count}`)
    }
  }, [count])

  return <button onClick={() => setCount((prev) => prev + 1)}></button>
}
```

从页面加载，到点击一次按钮，输出如下：

```bash
useEffect: 0
useEffect cleanup: 0
useEffect: 1
```
