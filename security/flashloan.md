# Flash Loans

Flash loans are not inherently dangerous.

They are atomic capital amplifiers.

The danger comes from:
- Broken invariants
- Oracle manipulation
- Incorrect assumptions about liquidity
- Temporary state inconsistency within a transaction

Flash loans magnify logic flaws.

---

# 1. Atomicity

Flash loans rely on Ethereum’s atomic transaction model.

Transaction rule:
If any step fails → entire transaction reverts.

Flash loan flow:

1. Borrow assets
2. Execute arbitrary logic
3. Repay principal + fee
4. If not repaid → revert

No collateral required because repayment is enforced by ato
