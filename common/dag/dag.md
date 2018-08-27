## common\dag\dag.go


### 功能描述
Dag(有向无环图)是为了解决block中打包的交易之间的依赖关系，在并发执行时能保证Block中的交易能够按照相同的顺序执行。

在Block的打包tx过程中，每个tx用DAG的一个Node（节点/顶点）来表示，Node的key是tx-hash。
如果某个tx-A与已打包的tx-B存在依赖关系（即两个tx都更改了某个account的状态），那么该tx-A将会成为tx-B的子节点。
其他nebulas节点验证block时，会从父顶点数为0的顶点（根顶点）开始执行，并且如果有多个根顶点，这些顶点可以并发执行。
这样就保证了存在依赖关系的交易会按照相同的顺序执行。

其主要功能有:

-

### 主要数据结构
DAG使用邻接链表法来描述.
```golang

// Dag 顶点的结构
type Node struct {
	key           interface{}   //这里是tx-hash
	index         int           //node的序号
	children      []*Node       //邻接顶点(子顶点)slice
	parentCounter int           //父顶点的数目
}

// Dag struct
type Dag struct {
	nodes  map[interface{}]*Node    //DAG中所有顶点
	index  int                      //当前顶点中的最大index
	indexs map[int]interface{}      //map[index]key
}

```
