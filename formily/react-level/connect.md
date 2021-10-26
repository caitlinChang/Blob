### connect

connect 的官方描述是

> 主要用于对第三方组件库的无侵入接入 `Formily`；

connect 与 mapProps 搭配来使用，connect 的第一个参数是要接入的组件，后面的参数是组件映射器， mapProps 就是一个组件映射器；

mapProps 的作用，将 Field 中的属性映射到组件的 props 上，即 Field 模型中的属性发生改变时，就会触发 props 的变化，然后触发组件的重新渲染；

mapReadPretty 的作用也是类似，通过 mapReadPretty 的第一个参数接受 read-pretty 状态下展示的组件；mapReadPretty 内部的逻辑就是通过读取 Field 的 pattern === 'readPretty' 来判断应该展示原组件还是阅读态组件；

> forwardRef: true 的作用？？？

connect 接收的第一个参数是原组件，后面的参数时组件映射器，connect 内部将组件映射器返回的值组合成最终的组件，并返回；
