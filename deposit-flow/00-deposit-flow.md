# Deposit Flow: L1 -> L2

## Полный flow

```text
User on L1
-> L1TokenBridge.bridgeERC20(...) / bridgeERC20To(...)
-> L1TokenBridge._initiateBridgeERC20(...)
-> check bridge is open
-> check L1 token maps to correct L2 token
-> transferFrom(user, escrow, amount)
-> messenger.sendMessage(...)
-> L2 messenger relays message
-> L2TokenBridge.finalizeBridgeERC20(...)
-> onlyOtherBridge check
-> mint(to, amount) on L2
```

## Простой смысл

```text
L1 токены блокируются в Escrow.
L2 токены минтятся получателю.
```

## Главная формула

```text
L1 escrowed amount = L2 minted amount
```

## Main Invariants

```text
1. L1 escrowed amount must equal L2 minted amount.
2. Only an authentic L1 -> L2 message can mint L2 tokens.
3. The L1 token must map to the correct L2 token.
```
