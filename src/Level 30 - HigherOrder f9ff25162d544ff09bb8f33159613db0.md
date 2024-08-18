# Level 30 - HigherOrder

- registerTreasury function accepts uint8 as argument, but treasury is 256bit integer. Updates treasury variable with assembly.
- We can manipulate calldata and send a value higher than 255. If we directly call with an integer > 8bit, it will assign integer to something < 255.
- Use this payload, `0x211c85ab20606e1500000000000000000000000000000000000000000000000000000001`
- First 4 bytes are function selector and rest is the 256 bit integer.
- Probably in this compiler version, there isn’t input type check. That way we have passed level. Otherwise, it shouldn’t accept an integer larger than 255 bit.