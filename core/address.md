## core/address


### 功能描述 
`address` 主要用于生成nebulas的账户地址.

其主要功能有:
- `Equals(...)` 判断address是否相等.
- `Type()` 返回address类型. 
- `NewAddressFromPublicKey` 根据公钥新建address. 
- `NewContractAddressFromData` 新建contract address,
- `AddressParseFromBytes` 验证给定的[]byte 是否为合法的address.

### 主要数据结构
```golang
type Address struct {
	address byteutils.Hash
}
```
