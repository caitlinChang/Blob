react 组件的渲染原理

1. 父组件重新 render，会触发子组件重新 render，在自组件 render 计算完成之前，不会渲染
2. 兄弟组件之间相互更新不会影响到
