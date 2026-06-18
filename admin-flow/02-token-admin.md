# Function Review: Token Admin Functions

## Function Code

```solidity
function registerToken(address l1Token, address l2Token) external auth {
    l1ToL2Token[l1Token] = l2Token;
    emit TokenSet(l1Token, l2Token);
}

function setMaxWithdraw(address l2Token, uint256 maxWithdraw) external auth {
    maxWithdraws[l2Token] = maxWithdraw;
    emit MaxWithdrawSet(l2Token, maxWithdraw);
}

function close() external auth {
    isOpen = 0;
    emit Closed();
}
```

Note:

```text
Original source:
makerdao/op-token-bridge/src/L1TokenBridge.sol
makerdao/op-token-bridge/src/L2TokenBridge.sol
```

## Что делают эти функции

Это admin functions, которые контролируют bridge configuration.

`registerToken(...)` задает L1 token -> L2 token mapping.

`setMaxWithdraw(...)` задает maximum withdrawal amount для L2 token.

`close(...)` disables new outbound bridge messages.

## Важные моменты логики

### Token Mapping

```solidity
l1ToL2Token[l1Token] = l2Token;
```

Это определяет, какой L2 token соответствует L1 token.

### Withdrawal Limit

```solidity
maxWithdraws[l2Token] = maxWithdraw;
```

Это ограничивает withdrawal size для token на L2.

### Close Bridge

```solidity
isOpen = 0;
```

Это prevents new bridge messages from being initiated.

Old in-flight messages may still need to finish.

## Main Invariants

```text
1. Only authorized governance/admin can change bridge configuration.
```
