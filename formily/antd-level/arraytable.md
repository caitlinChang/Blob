### array base

ArrayBase 是一个维护 array 类型数据状态的组件，而其他的 ArrayTable，ArrayCollapse 和 ArrayItems、ArrayCards 无非是套了一层不同的 UI 壳子;

### 疑问：

arrayBase 是怎么做到给数据排序的功能的？

### array table 如何将 column 映射到 schema 的

```javascript
const schema = {
    type:'object',
    properties:{
        table:{
            type:'array',
            'x-component':'ArrayTable',
            items:{
                column1:{
                    type:'void',
                    'x-component':'ArrayTable.Column',
                    properties:{
                        index:{
                            type:'void',
                        }
                    }
                },
                column2:{
                    type:'void',
                    'x-component':'ArrayTable.Column'
                    properties:{
                        name:{
                            type:'string',
                            'x-component':'Input'
                        },
                    }
                },
                column3: {
                    type: 'void',
                    'x-component': 'ArrayTable.Column',
                    'x-component-props': {
                        title: 'Operations',
                        dataIndex: 'operations',
                        width: 200,
                        fixed: 'right',
                    },
                    properties: {
                        item: {
                            type: 'void',
                            'x-component': 'FormItem',
                            properties: {
                                remove: {
                                    type: 'void',
                                    'x-component': 'ArrayTable.Remove',
                                },
                                moveDown: {
                                    type: 'void',
                                    'x-component': 'ArrayTable.MoveDown',
                                },
                                moveUp: {
                                    type: 'void',
                                    'x-component': 'ArrayTable.MoveUp',
                                },
                            },
                        },
                    },
                }
            }
        }
    }
}
```
```javascript

const ArrayTable = (props) => {
    const columnsSchema = useSchema();
    const columns = []
    return <Table columns={columns}></Table>
}
const schema = useSchema();
const items = Array.isArray(schema.items) ? schema.items[0] : schema.items;


```