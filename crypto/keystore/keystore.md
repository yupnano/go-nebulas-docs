## crypto/keystore/keystore.go

### 功能描述 
`keystore` 用来存储account.manager管理的accounts (这里是用map存储的, key是address, value是加密后的私钥信息).

其主要功能有:
- 对保存的account进行增删改查: keystore中的account保存在'keystore.p' (memory_provider)中. 
- 解锁/锁定账户: 解锁后的账户保存在map `keystore.unlocked` 中, 并用gorutinue执行 `keystore.expire` 来定时锁定已解锁的账户.
- 管理解锁的账户.

### 主要数据结构
```golang
// Keystore class represents a storage facility for cryptographic keys
type Keystore struct {

	// keystore provider
	p Provider

	// unlocked items
	unlocked map[string]*unlocked

	mu sync.RWMutex
}
```
其中 
- `p Provider` 是 keystore 用来存储账户数据的具体执行单元.
- `unlocked map[string]*unlocked` 是用来临时管理解锁的账户, 暂时保存 private key 明文数据.
- `mu sync.RWMutex` 是读写互斥锁.


```
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
`MemoryProvider` :
- `entries map[string]Entry` 保存account信息, 是account manager 管理的账户的存储单元.
- `cipher *cipher.Cipher` 加密私钥的加密算法, 目前只有`SCRYPT`算法, 在常量 

```
// Provider class represents a "provider" for the
// Security API, where a provider implements some or all parts of
// Security. Services that a provider may implement include:
// Algorithms
// Key generation, conversion, and management facilities (such as for
// algorithm-specific keys).
// Each provider has a name and a version number, and is configured
// in each runtime it is installed in.
type Provider interface {

	// Aliases all alias in provider save
	Aliases() []string

	// SetKey assigns the given key (that has already been protected) to the given alias.
	SetKey(a string, key Key, passphrase []byte) error

	// GetKey returns the key associated with the given alias, using the given
	// password to recover it.
	GetKey(a string, passphrase []byte) (Key, error)

	// Delete remove key
	Delete(a string) error

	// ContainsAlias check provider contains key
	ContainsAlias(a string) (bool, error)

	// Clear all entries in provider
	Clear() error
}


```