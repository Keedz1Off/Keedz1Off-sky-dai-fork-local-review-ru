# Admin / Escrow Break Think

## Governance / Admin Control

```text
INVARIANT
Only authorized governance/admin can change bridge configuration.

CONSEQUENCES
An unauthorized user may change token mapping, withdrawal limits, bridge status, or Escrow approvals.
```

## Escrow.approve(...)

```text
INVARIANT
Only authorized governance/admin can approve token spending from Escrow.
```

## Token admin functions

```text
INVARIANT
Only authorized governance/admin can register tokens, set maxWithdraw, or close the bridge.
```
