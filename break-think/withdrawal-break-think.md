# Withdrawal Break Think

## L2TokenBridge.bridgeERC20(...)

```text
INVARIANT
The intended L1 recipient must be preserved.

CONSEQUENCES

1. L2 tokens may be burned, but L1 tokens may be released to the wrong recipient.
```

## L2TokenBridge.bridgeERC20To(...)

```text
INVARIANT
The custom L1 recipient must be preserved.

CONSEQUENCES

1. L2 tokens may be burned, but L1 tokens may be released to the wrong custom recipient.
```

## L2TokenBridge._initiateBridgeERC20(...)

```text
INVARIANT
L2 burned amount must equal L1 released amount.
The L2 token must map to the correct L1 token.
The withdrawal message must be created only after the burn step.

CONSEQUENCES

1. The bridge may release more L1 tokens than were actually burned on L2.
2. A user may receive the wrong L1 token.
3. This may lead to L1 release without a valid L2 burn.
```

## L1TokenBridge.finalizeBridgeERC20(...)

```text
INVARIANT
Only an authentic L2 -> L1 message can release L1 tokens.
Released amount must equal the L2 burned amount.
Released token must be the correct L1 token for the L2 token.

CONSEQUENCES

1. This may lead to spoofed message execution and fake finalization.
2. The bridge may release more tokens than were actually burned on L2.
3. A user may receive the wrong L1 token.
```
