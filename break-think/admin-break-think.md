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

CONSEQUENCES
An unauthorized spender may get approval and move tokens from Escrow.
```

## L1TokenBridge.registerToken(...)

```text
INVARIANT
Only authorized governance/admin can register token pairs.

CONSEQUENCES
An unauthorized user may register a wrong token pair.
```

## L2TokenBridge.registerToken(...)

```text
INVARIANT
Only authorized governance/admin can register token pairs.

CONSEQUENCES
An unauthorized user may register a wrong token pair.
```

## L2TokenBridge.setMaxWithdraw(...)

```text
INVARIANT
Only authorized governance/admin can set maxWithdraw.

CONSEQUENCES
An unauthorized user may change withdrawal limits.
```

## L1TokenBridge.close(...)

```text
INVARIANT
Only authorized governance/admin can close the L1 bridge.

CONSEQUENCES
An unauthorized user may stop new L1 -> L2 messages.
```

## L2TokenBridge.close(...)

```text
INVARIANT
Only authorized governance/admin can close the L2 bridge.

CONSEQUENCES
An unauthorized user may stop new L2 -> L1 messages.
```
