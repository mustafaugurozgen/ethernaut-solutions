# Level 28 - Gatekeeper Three

- We need to call enter and pass three modifiers.
- First thing I have realized is that constructor of Gatekeeper contract. There isn’t any constructor actually. It is defined as public function.
- ***Gate one*** is simple. We will create a contract. Make it owner of the Gatekeeper by calling construct0r first. Then we will make calls via this contract. So that we will be tx.origin and contract will be msg.sender.
- ***Gate two*** requires allowEntrance to be true. It is false initially. We have to call getAllowance with correct password.
    - This method calls checkPassword method of Trick contract.
    - We need to get address of SimpleTrick contract first but it is not created yet. First we have to create it with createTrick method.
    - Due to storage optimization, owner is stored in slot 0. Entrant and AllowEntrance are stored in slot 1 together. So address of trick contract is at slot 2. We can read its address with `web3.eth.getStorageAt(instance,2)`
    - For trick contract, password is stored in storage 2. We can read password same way.
- ***Gate three*** requires balance of gatekeeper contract to be more than 0.001 and tries to send 0.001 ether to owner address. However, it should return false. So we can easily handle this in our attacker contract. If we don’t implement receive, it will return false.
- What we have to do before deploying our contract is sending some eth to Gatekeeper and set allowance to true with password.
- Attacker contract:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface Gatekeeper {
    function construct0r() external ;
    function enter() external ;
}

contract Attacker {

    function attack(address _target) public {
        Gatekeeper(_target).construct0r(); // Call construct0r() to become Owner
        Gatekeeper(_target).enter(); // Call enter()
    }
}
```