# Level 19 - Alien Codex

- [Source1](https://ethereum.stackexchange.com/questions/140776/where-inside-smart-contract-storage-are-dynamic-array-values-stored), [Source2](https://programtheblockchain.com/posts/2018/03/09/understanding-ethereum-smart-contract-storage/), [Source3](https://weka.medium.com/announcing-the-winners-of-the-first-underhanded-solidity-coding-contest-282563a87079)

<aside>
📨 Assuming the dynamic array starts at a slot location `p`, then the slot `p` will contain the total number of elements stored in the array, and the actual array data will be stored at `keccack256(p)`

</aside>

- In this example, first state variable is owner since it is inherited from Ownable contract. Second state variable is boolean and third is bytes32 dynamic array.
- In this case, storage is as follows:
    - Slot 0 → 0x000000.. + boolean + owner address
    - Slot 1 → Length of dynamic array
    - Slot 2 → Empty ..
- Dynamic array data is stored as follows:
    - hash(length slot) + index*bit/256
    - First element → hash(1) + 0*256/256
    - Second element → hash(1) + 1
    - …
- In this case, we attack revise method to change data of first slot.
    - Before all, we decrease length by 1, and underflows to max value. That way we can access any data in array.
    - First, calculate storage point of first element of array. `web3.utils.keccak256(web3.utils.encodePacked(1))`
    - There can be 2^256 slots in storage. Last storage address is 0xffff…ffff.
    - If we give index as 2^256 - first element address, we overwrite to slot 0.
- There we write our address to that index of array. It will overwrite slot 0.