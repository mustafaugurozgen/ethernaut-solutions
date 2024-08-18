# Level 8 - Vault

- Nothing is secret
- Password is passed as parameter in constructor but I couldn’t find the value that way.
- In EVM storage, data is stored as 32 byte slots. and from right to left. Slot number starts from 0.
- In our example, first variable is bool and second one is bytes32. Bool is 1 byte. It is located in first slot. 32 byte value is stored in second slot.
- We can see its value by directly accessing the storage.
- `web3.eth.getStorageAt(instance, 1)`