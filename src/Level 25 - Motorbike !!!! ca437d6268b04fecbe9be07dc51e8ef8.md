# Level 25 - Motorbike !!!!

- NOT SOLVABLE because of latest updates about selfdestruct. Now, selfdestruct doesn’t destruct the code after it is deployed, only transfers balance. It only selfdestructs if selfdestruct is called in constructor.
- There are two issues in this question:
    - When we update contract _upgradeToAndCall function, it updates the storage with address but it also sends delegatecall right after updated. If we upgrade it to a contract with selfdestruct implemented, we can make it call with delegatecall. And it will destruct itself.
    - Second problem is that, initialize function is public to anyone. Since it is initialized via a delegatecall from proxy, initialization library states are store in proxy storage. So if we directly call init function of Engine without proxy, we can set ourselves as upgrader in context of Engine. Then we can call upgrade function same way.
- [https://dev.to/erhant/ethernaut-25-motorbike-25b4](https://dev.to/erhant/ethernaut-25-motorbike-25b4)

| Instance | 0x5e47823c2B28Fe2220C368EF3FbF4Fb55A475171 |  |
| --- | --- | --- |
| Slot 0 | 0x00000000000000000000a67972265516e4bfea3d4f9c70749768be2d29f80001 | Level Address + initializer library variables  |
| Slot 1 | 0x00000000000000000000000000000000000000000000000000000000000003e8 | 1000 |
| Slot 2 | 0 |  |
| Slot impl.  | 0x0000000000000000000000007671d602e13e910ecc6b0ff66e0a5c4c44711f26 | Engine Address |