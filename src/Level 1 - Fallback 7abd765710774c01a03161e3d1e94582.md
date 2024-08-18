# Level 1 - Fallback

- `contract.contribute({value: toWei("0.0001")})`
- `contract.send(toWei("0.0001"))`
- BN object type → Big number. JS does not support 256bit integers. For this reason uint256 responses are stored in BN objects. In order to get its value, use toString.
- An example → `(await contract.getContribution()).toString()`