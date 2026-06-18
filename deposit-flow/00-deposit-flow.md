# Deposit Flow: L1 -> L2

Этот файл показывает общий deposit flow для Sky OP Token Bridge.

## High-Level Flow

```text
User on L1
-> L1TokenBridge.bridgeERC20(...) / bridgeERC20To(...)
-> L1TokenBridge._initiateBridgeERC20(...)
-> transferFrom(user, escrow, amount)
-> messenger.sendMessage(...)
-> L2TokenBridge.finalizeBridgeERC20(...)
-> mint(to, amount)
```

## Простое объяснение

Пользователь начинает deposit на L1.

L1 bridge забирает токены пользователя и отправляет их в Escrow.

После этого L1 bridge отправляет cross-chain message на L2.

L2 bridge получает сообщение и mint токены получателю.

```text
L1 lock/escrow -> L2 mint
```

## Main Invariants

```text
1. L1 escrowed amount must equal L2 minted amount.
2. Only an authentic L1 -> L2 message can mint L2 tokens.
3. The L1 token must map to the correct L2 token.
```

## Additional Invariants / Checks

```text
The bridge must be open before sending a new deposit message.
The recipient must be the intended recipient.
The message must target the correct L2 bridge.
The encoded calldata must match the real L1 deposit.
```

## Functions In This Flow

```text
01-l1-bridgeERC20.md
02-l1-initiateBridgeERC20.md
03-l2-finalizeBridgeERC20.md
```
