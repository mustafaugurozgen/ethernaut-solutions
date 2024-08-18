# Level 23 - Dex Two

> As we've repeatedly seen, interaction between contracts can be a source of unexpected behavior.
> 
> 
> Just because a contract claims to implement the [ERC20 spec](https://eips.ethereum.org/EIPS/eip-20) does not mean it's trust worthy.
> 
> Some tokens deviate from the ERC20 spec by not returning a boolean value from their `transfer` methods. See [Missing return value bug - At least 130 tokens affected](https://medium.com/coinmonks/missing-return-value-bug-at-least-130-tokens-affected-d67bf08521ca).
> 
> Other ERC20 tokens, especially those designed by adversaries could behave more maliciously.
> 
> If you design a DEX where anyone could list their own tokens without the permission of a central authority, then the correctness of the DEX could depend on the interaction of the DEX contract and the token contracts being traded.
> 
- Contract trust on any ERC20 token.
- We implement a ERC20 token and send 1 token to dex. Keep rest.
- If we want to swap 1 malicious token with any other token, swap amount will be 100.
- That way we can drain.
- Token contract below. Regular ERC20 contract.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC20/ERC20.sol";

contract SwappableTokenTwo is ERC20 {
    address private _dex;

    constructor(address dexInstance, string memory name, string memory symbol, uint256 initialSupply)
        ERC20(name, symbol)
    {
        _mint(msg.sender, initialSupply);
        _dex = dexInstance;
    }

    function approve(address owner, address spender, uint256 amount) public {
        require(owner != _dex, "InvalidApprover");
        super._approve(owner, spender, amount);
    }
}

```