优先级列表，就是在插入链表时，要考虑链表元素的某个特征值。在内核中优先级列表由两个双链表组合实现。

# 单个的样子

```
    plist_node
    +-----------------------+
    |prio                   |
    |    (int)              |
    |prio_list              |
    |node_list              |
    |    (struct list_head*)|
    +-----------------------+
```

其中

* prio表示元素的优先级
* prio_list是优先级链表元素
* node_list是整个链表元素

乍一看觉得为啥要两个链表，从下面整体的图上会看得比较清楚。

# 整体的样子

内核开发者给优先级列表画了图：

```
pl:prio_list (only for plist_node)
nl:node_list
  HEAD|             NODE(S)
      |
      ||------------------------------------|
      ||->|pl|<->|pl|<--------------->|pl|<-|
      |   |10|   |21|   |21|   |21|   |40|   (prio)
      |   |  |   |  |   |  |   |  |   |  |
      |   |  |   |  |   |  |   |  |   |  |
|->|nl|<->|nl|<->|nl|<->|nl|<->|nl|<->|nl|<-|
|-------------------------------------------|
```

意思是

* prio_list是链接没有优先级中第一个元素的链表
* node_list则是所有的元素都会链接的链表

不知道是不是讲清楚了，不过实现上就是这么有意思。

# 常用API

* plist_add()
* plist_del()

主要就是这两个，没啥花花肠子。