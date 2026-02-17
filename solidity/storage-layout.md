# Storage Layout in Solidity

Understanding storage layout is mandatory for:
- Gas optimization
- Upgradeable contracts
- Low-level debugging
- Exploit surface analysis

This document focuses on slot computation, packing rules, and edge cases.

---

# 1. EVM Storage Model

- Storage is a key-value store
- Key: 32-byte slot (uint256)
- Value: 32 bytes
- Addressed by 256-bit index

Storage access opcodes:
- SLOAD
- SSTORE

Cost (approximate):
- SLOAD: ~100 gas (warm) / 2100 gas (cold)
- SSTORE: 20,000 gas (new) / 5,000 gas (update)

---

# 2. Slot Allocation Rules

Solidity assigns storage sequentially starting at slot 0.

Each slot = 32 bytes.

## Rule 1: Value Types (<= 32 bytes)

Packed into the same slot if:
- Declared consecutively
- Combined size ≤ 32 bytes

Example:

```solidity
uint128 a;   // 16 bytes
uint128 b;   // 16 bytes
