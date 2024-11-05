##### Solidity string storage 
- 32 non-zero bytes for Solidity native strings costs more gas than 31 non-zero bytes
##### - In EVM, strings are stored in two different patterns:
1. For strings <= 31 bytes (short strings):
- A single 32-byte storage slot is used
- The first byte of the slot is used to store length information and a flag indicating if it's a short string
- First byte stores length*2 (even number, indicating short string)
- Remaining 31 bytes store the actual string data

2. For strings > 31 bytes (long strings):
- The first storage slot stores length*2 + 1 (odd number, indicating long string)
- Rest of this slot is padded with zeros
- The actual string data is stored in slots starting at keccak256(p), where p is the storage position
- This storage pattern transition and hash-based computation causes a gas cost jump

