# Reentrancy

Reentrancy is not just "calling back before state update".
It is control-flow re-entry into inconsistent state.

To understand reentrancy, you must understand:

- EVM call semantics
- Call stack behavior
- State write ordering
- External control transfer

---

# 1. EVM Call Semantics

When a contract executes:

- CALL
- DELEGATECALL
- CALLCODE
- STATICCALL

Control is transferred to another contract.
Execution resumes only after the call returns.

During that external call:
- The callee has full control.
- It can call back into the caller.
- Storage changes made before the call persist.

This is the core attack surface.

---

# 2. Classic Reentrancy (Single Function)

Example vulnerable pattern:

```solidity
function withdraw(uint256 amount) external {
    require(balance[msg.sender] >= amount);

    (bool ok, ) = msg.sender.call{value: amount}("");
    require(ok);

    balance[msg.sender] -= amount;
}
