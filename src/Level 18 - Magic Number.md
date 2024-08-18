# Level 18 - Magic Number

- [Source1](https://medium.com/coinmonks/develop-evm-assembly-opcode-logic-for-fibonacci-107f92dbc9d1), [Source2](https://medium.com/@blockchain101/solidity-bytecode-and-opcode-basics-672e9b1a88c2), [Source3](https://medium.com/@kalexotsu/writing-evm-logic-in-opcodes-deploying-opcode-logic-on-chain-205618fee38d)
- [https://www.evm.codes/playground](https://www.evm.codes/playground)
- [https://dev.to/nvnx/ethernaut-hacks-level-18-magic-number-27ep](https://dev.to/nvnx/ethernaut-hacks-level-18-magic-number-27ep)
- Return value should be 32 bytes
- Runtime Bytecode → `60 2a 60 00 52 60 20 60 00 f3`
- Initialization Bytecode → `0x600a600c600039600a6000f3602a60005260206000f300000000000000000000`
- Contract that deploys bytecode:

```solidity
pragma solidity >= 0.8.0;

contract MagicNumberAttacker {
    function deployExploiterContract() public returns(address contractAddress) {
        assembly {
            let ptr := mload(0x00)
            mstore(ptr, 0x600a600c600039600a6000f3602a60005260206000f300000000000000000000)

            contractAddress := create(0, ptr, 22)
        }
    }
}
```