### FormDialog

#### 如何在 schema 中使用弹框

```JSX
<SchemaField schema={schema} />
```

#### dialog 使用场景

我们遇到的场景，点击按钮，查询表单内容，弹出表单，编辑，点击 save 按钮去校验；
dialog 的实现没有在 schema 里面设计，因为有太多的事件要处理；

> `formily`对 dialog 的封装就是对弹框内的内容
> form

#### FormDialog 提供的钩子

**forOpen**
**forConfirm**
**forCancel**

**open**
**then**
**catch**

1. 其实是在 schema 情况下一种使用 dialog 的方式
2. FormDialog.content 是一个表单
3.

### FormDialog.Portal

通过 id 来控制唯一弹框

```JSX
<FormDialog.Portal>
    {/* portal 中增加了一部分逻辑是用来创建弹框的 */}
</FormDialog.Portal>
```

### FormDialog

middleware 中间件改造 props

```typescript

```
