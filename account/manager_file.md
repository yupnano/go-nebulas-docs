## account/manager_file

### 功能描述 
`manager_file` 提供了处理account keystore文件相关的接口, 这些接口都是`manager`结构体的方法. 

其主要功能有:
- 更新manager keydir目录下的所有账户到`manager.accounts`中. 当新建manager或读取manager下所有账户的时候会被执行一次.
- 导出keystore文件到keydir: 每当新建account/更新account密码/由keyJson导入account 时都会被执行.
- 更新account的keydir.
