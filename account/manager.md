## account/manager

### 功能描述 
`manager`用于管理当前neb的accounts, 它实现了types中定义的`AccountManager`接口.
 所有账户的私钥信息加密后存储在keystore中,需要使用提供密码才能操作某个account.

其主要功能有:
- 新建manager: 新建一个account manager. 当节点启动的时候会创建一个manager用于管理当前节点的所有accounts.

- 创建account: 根据提供的密码创建新的account,这里的密码是用于加密account.
- 解锁/锁定account: 当使用account进行签名操作时,需要先输入密码来得到私钥. 解锁功能可以把私钥临时保存在一个map中,不需要每次都输入密码.
- 更新account密码
- 返回当前所有的accounts
- 导入/导出keyJson文件
- 对hash\transaction\block进行签名: 使用已解锁的account的私钥根据 `manager.signatureAlg"指定的算法进行签名, 目前仅支持SECP256K1算法.
- 生成VRF的随机数种子



### 主要数据结构

```golang
// Manager accounts manager ,handle account generate and storage
type Manager struct {

	// keystore
	ks *keystore.Keystore

	// key save path
	keydir string

	// key encrypt alg
	encryptAlg keystore.Algorithm

	// key signature alg
	signatureAlg keystore.Algorithm

	// account slice
	accounts []*account

	mutex sync.Mutex
}
```

- `ks *keystore.Keystore` 用于存储 manager 管理的账户信息, 主要保存加密后的私钥数据. 参见 `crypto/keystore`
- `keydir string` manager 所管理的账户的keyStore文件的存储目录. 
- `encryptAlg keystore.Algorithm` 加密算法, 用于加密私钥, 目前仅支持`SCRYPT`.
- `signatureAlg keystore.Algorithm` 签名算法, 用于签名, 目前仅支持`SECP256K1`.
- `accounts []*account` 用于记录当前manager中所有account的slice. 


```golang
type account struct {
	// key address
	addr *core.Address

	// key save path
	path string
}
```
manager中存储的account结构, 包括账户address 和目录信息path

### 模块关系

