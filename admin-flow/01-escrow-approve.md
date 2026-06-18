# Function Review: Escrow.approve(...)

## Код функции

```solidity
function approve(address token, address spender, uint256 value) external auth {
    GemLike(token).approve(spender, value);
    emit Approve(token, spender, value);
}
```

## Что делает

`Escrow.approve(...)` дает spender разрешение тратить токены из Escrow.

В bridge context это нужно, чтобы L1 bridge мог release токены из Escrow при valid withdrawal.

## Main Invariant

```text
Only authorized governance/admin can approve token spending from Escrow.
```
