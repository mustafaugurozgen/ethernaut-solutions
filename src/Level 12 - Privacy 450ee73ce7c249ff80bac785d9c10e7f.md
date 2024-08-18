# Level 12 - Privacy

- data bytes32[3] is stored in storage level 3-4-5.
- `require(_key == bytes16(data[2]));` here data[2] belongs to storage 5.
- Casting to 16 bytes takes first 16 bytes.
- `contract.unlock("0x9822c3f80a13d702dc3c15ebce5ac0c0")`
- [https://medium.com/@dariusdev/how-to-read-ethereum-contract-storage-44252c8af925](https://medium.com/@dariusdev/how-to-read-ethereum-contract-storage-44252c8af925)