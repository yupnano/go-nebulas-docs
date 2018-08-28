## core\block.go


### 功能描述

- Block打包过程的一些方法，包括 SetTimestamp\CollectTransactions\Seal\RollBack
- block验证：执行block，并验证执行结果。

### 主要数据结构


```golang

// Block structure
type Block struct {
	header       *BlockHeader
	transactions Transactions   //blcok中打包的交易
	dependency   *dag.Dag       //交易之间的依赖关系DAG

	sealed bool
	height uint64       //block 高度

	worldState state.WorldState     //

	txPool       *TransactionPool   //
	eventEmitter *EventEmitter      //
	nvm          NVM
	storage      storage.Storage    //
}
```

### 流程图

Block打包transaction的流程

![Block打包transaction的流程](../resource/block-CollectTransactions.png)

Block验证的流程

![Block验证的流程](../resource/block-VerifyExecution.png)



