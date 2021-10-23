### formily 架构
#### @formily/core
`@formily/core` 提供动态表单的核心功能，Form 和 Field 的状态管理；
@formily/core是提供Form & Field 的 context，表单内部字段管理和联动逻辑的视线也是在这一层，核心是利用mobx；
所以@formily/core提供的API 是 
* createForm - 用于生成form实例
* createField - 用于生成 field 实例
* effect hooks 等


#### @formily/react
按照官方描述，`@formily/react`是胶水层；胶水层是连接组件和core的链接层；
@formily/react 到底提供了哪些能力呢，
首先就是 FormProvider, 用于给子组件提供context；主要是给自定义组件

本质上，用useForm/useField实现 core 与 UI 的绑定；
```

export const useForm = function(){

}

export const useField = function(){
  return field
}
```javascript

##### 

### schema
@formily/react 提供了 SchemaField 这样的协议驱动组件
 makeup schema -> json schema -> jsx -> formily/core



