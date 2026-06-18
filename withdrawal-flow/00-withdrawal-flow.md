# Withdrawal Flow: L2 -> L1

## Полный flow

```text
User on L2
-> L2TokenBridge.bridgeERC20(...) / bridgeERC20To(...)
-> L2TokenBridge._initiateBridgeERC20(...)
-> check bridge is open
-> check L2 token maps to correct L1 token
-> check amount <= maxWithdraw
-> burn(user, amount) on L2
-> messenger.sendMessage(...)
-> OP withdrawal proving / finalization delay
-> L1TokenBridge.finalizeBridgeERC20(...)
-> onlyOtherBridge check
-> transferFrom(escrow, to, amount)
```

## Главная формула

```text
L2 burned amount = L1 released amount
```

## Main Invariants

```text
1. L2 burned amount must equal L1 released amount.
2. Only an authentic L2 -> L1 message can release L1 tokens.
3. The L2 token must map to the correct L1 token.
```
