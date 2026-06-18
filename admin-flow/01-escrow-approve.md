# Function Review: Escrow.approve(...)

## Function Code

```solidity
function approve(address token, address spender, uint256 value) external auth {
    GemLike(token).approve(spender, value);
    emit Approve(token, spender, value);
}
```

Note:

```text
Original source: makerdao/op-token-bridge/src/Escrow.sol
```

## Что делает эта функция

`Escrow.approve(...)` дает spender approval двигать tokens из escrow.

В этом bridge L1 bridge releases tokens from escrow во время withdrawal finalization.

Простой смысл:

```text
Escrow holds L1 tokens.
Approved bridge can transfer tokens out when a valid withdrawal is finalized.
```

## Важные моменты логики

### Auth

```solidity
external auth
```

Только authorized address может approve token spending from escrow.

### Token Approval

```solidity
GemLike(token).approve(spender, value);
```

Это задает ERC20 allowance from Escrow to the spender.

## Main Invariants

```text
1. Only authorized governance/admin can approve token spending from Escrow.
```
