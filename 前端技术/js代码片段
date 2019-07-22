# 代码片段

## 数组转树状结构数据

```
 arrToTrees = (items) => {
    for (let i = 0; i < items.length; i++) {
      for (let j = 0; j < items.length; j++) {
        //parentId为root表示没有父节点  即根节点
        if (items[i].parentId !== 'root' && items[i].parentId === items[j].id) {
          if (!(items[j].children instanceof Array)) {
            //children初始化
            items[j].children = [];
          }
          items[j].children.push(items[i]);
        }
      }
    }
    //返回所有的根节点构成的森林
    return items.filter((item) => (item.parentId === 'root'));
  };  
```
