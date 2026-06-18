# Function Review: Token Admin Functions

## Код функций

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

## Что делают

Это admin/governance функции для настройки bridge.

```text
registerToken(...)  = добавить token pair
setMaxWithdraw(...) = поставить лимит withdrawal
close(...)          = закрыть новые bridge messages
```

## Main Invariant

```text
Only authorized governance/admin can change bridge configuration.
```
