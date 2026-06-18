# Admin / Escrow Break Think

## Governance / Admin Control

```text
ИНВАРИАНТ
Только авторизованный governance/admin может менять конфигурацию bridge.

ПОСЛЕДСТВИЯ
Неавторизованный пользователь может изменить token mapping, withdrawal limits, bridge status или Escrow approvals.
```

## Escrow.approve(...)

```text
ИНВАРИАНТ
Только авторизованный governance/admin может approve расходование токенов из Escrow.
```

## Token admin functions

```text
ИНВАРИАНТ
Только авторизованный governance/admin может register tokens, set maxWithdraw или close bridge.
```
