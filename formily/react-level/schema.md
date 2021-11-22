### schema 是如何被渲染成表单的？

#### SchemaField 组件

SchemaField 组件的作用是，识别 Markup Schema 和 JSON Schema

```JSX
export function createSchemaField<Components extends SchemaReactComponents>(
  options: ISchemaFieldReactFactoryOptions<Components> = {}
) {
  // SchemaField 的定义
  function SchemaField<
    Decorator extends JSXComponent,
    Component extends JSXComponent
  >(props: ISchemaFieldProps<Decorator, Component>) {
    // 先把 schema 实例化， 确保下层的schema拥有 Schema 类的方法
    const schema = Schema.isSchemaInstance(props.schema)
      ? props.schema
      : new Schema({
          type: 'object',
          ...props.schema,
        })

    // renderMarkup 用来解析 markup schema
    const renderMarkup = () => {
      env.nonameId = 0
      // 如果props中已经传入了schema，就会不渲染 props.children了
      // 就是 JSON Schema 和 markup Schema 不能共用
      // 其实共用的话，会不知道将markup schema 插入到json schema的哪一节点去❓
      if (props.schema) return null
      // 渲染 MarkupField, 并提供 parent context
      return render(
        <SchemaMarkupContext.Provider value={schema}>
          {props.children}
        </SchemaMarkupContext.Provider>
      )
    }
    // renderChildren才是真正解析JSON Schema 的地方
    // RecursionField 把json schema 转化成 Field JSX
    const renderChildren = () => {
      return <RecursionField {...props} schema={schema} />
    }

    return (
      <SchemaOptionsContext.Provider
        value={{
          ...options,
          components: {
            ...options.components,
            ...props.components,
          },
        }}
      >
        <SchemaExpressionScopeContext.Provider
          value={{
            ...options.scope,
            ...props.scope,
          }}
        >
          {renderMarkup()}
          {renderChildren()}
        </SchemaExpressionScopeContext.Provider>
      </SchemaOptionsContext.Provider>
    )
  }

```

#### makeup schema 的实现

SchemaField 中定义了 SchemaFiled.String、SchemaField.Number 等组件，它们的实现方式是通过 MarkupField 实现的

```typescript
// 解析 markup schema
const MarkupField = function(props:{
type: SchemaType;
...SchemaProps
}){
  // 通过SchemaMarkupContext 来维护每一个schema节点的parent
  const parent = useContext(SchemaMarkupContext)
  if(!parent) return null
  schema = parent.addProperty(name,resProps)// 生成新的schema
  // 这里需要对parent做一个判断，需要执行parent.addproperty来生成新的schema，
  // markup schema transter into json schema
  const { type, parent, name, ...resProps } = props

  if(type === 'object'){


  }if(type === 'array'){

  }else {
    // 如果是 string、number 这种叶子结点， 就直接渲染 RecursionField
    return <RecursionField>
  }
  return <SchemaMarkupContext.Provider><SchemaMarkupContext.Provider/>
}
```

总结一下思想

### !!! RecursionField 如何去渲染 array 数据并且塑造 key 值的
