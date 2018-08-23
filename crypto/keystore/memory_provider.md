## crypto/keystore/memory_provider.go

### 功能描述 
`MemoryProvider` 实现了 `Provider`接口.

其主要功能有:
- `Aliases()` 获取当前provider中保存的所有account列表. 这里的alias是账户address.
- `Clear()` 清空当前provider中保存的accounts.
- 对provider中保存的accounts进行增删改查操作.

### 主要数据结构

```golang
// MemoryProvider handle keystore with ecdsa
type MemoryProvider struct {

	// name of ecdsa provider
	name string

	// version of ecdsa provider
	version float32

	// a map storage entry
	entries map[string]Entry

	// encrypt key
	cipher *cipher.Cipher

	mu sync.RWMutex
}
```

- `entries map[string]Entry` map结构, key是address, value是entry结构, 主要是加密后的私钥信息.
-  `cipher *cipher.Cipher` 用于加密私钥的加密算法.

```golang
// Entry keeps in memory
type Entry struct {
	key  Key
	data []byte
}
```