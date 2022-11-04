### 状态流管理

一个组件的 state 发生改变时，它的 render 函数就会重新执行；同时，它的子组件也会重新执行，不管子组件有没有用到这个变化的 state;
这就是大表单场景下有联动逻辑时页面卡顿的原因；[demo](https://codesandbox.io/s/da-biao-dan-antd-demo-kd56nf)
