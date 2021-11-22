灵魂三问：

- 什么是 ref
- 用来做什么
- 经常使用的场景
- 有什么副作用

### forwardRef

该方法的意思是转发引用
用 forwardRef 包裹的组件可以接受一个 ref 参数，用来将组件内部的某些状态或方法暴露出去

```JSX
const ComTest = React.forwardRef((props,ref) => {
    return <button ref={ref}></button>
});

const MainTest = () => {
    const btnRef = useRef();

    return <ComText ref={btnRef}> />
}
```
