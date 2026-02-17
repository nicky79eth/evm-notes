# Gas Optimization in Solidity

Gas optimization is not about micro-tricks.
It is about understanding:

- EVM cost model
- Storage vs memory trade-offs
- Cold vs warm access
- Refund mechanics
- Bytecode size impact

This document focuses on structural optimizations that matter in production.

---

# 1. EVM Cost Model Overview

Main cost drivers:

- SSTORE
- SLOAD
- External calls
- LOG opcodes
- Contract deployment (bytecode size)

Post Berlin (EIP-2929):
- First storage access = cold (expensive)
- Subsequent access in same tx = warm (cheaper)

Implication:
Repeated reads of same slot are significantly cheaper after first access.

---

# 2. Storage vs Memory

Storage = persistent, expensive  
Memory = temporary, cheap  
Calldata = cheapest (read-only)

## Rule of Thumb

- Read from calldata when possible
- Cache storage variables into memory if used multiple times

Example (bad):

```solidity
function foo() external {
    uint256 x = value;
    if (value > 10) { ... }
}
