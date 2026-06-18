# Withdrawal Flow: L2 -> L1

Этот файл показывает общий withdrawal flow для Sky OP Token Bridge.

## High-Level Flow

```text
User on L2
-> L2TokenBridge.bridgeERC20(...) / bridgeERC20To(...)
-> L2TokenBridge._initiateBridgeERC20(...)
-> burn(user, amount)
-> messenger.sendMessage(...)
-> prove/finalize withdrawal through OP system
-> L1TokenBridge.finalizeBridgeERC20(...)
-> transferFrom(escrow, to, amount)
```

## Простое объяснение

Пользователь начинает withdrawal на L2.

L2 bridge сжигает user's L2 tokens.

После этого L2 bridge отправляет сообщение на L1.

L1 bridge получает authentic withdrawal message и выпускает L1 tokens из Escrow.

```text
L2 burn -> L1 release
```

## Main Invariants

```text
1. L2 burned amount must equal L1 released amount.
2. Only an authentic L2 -> L1 message can release L1 tokens.
3. The L2 token must map to the correct L1 token.
```

## Additional Invariants / Checks

```text
The bridge must be open before sending a new withdrawal message.
The withdrawal amount must not exceed maxWithdraw.
The recipient must be the intended recipient.
The encoded calldata must match the real L2 burn.
```

## Functions In This Flow

```text
01-l2-bridgeERC20.md
02-l2-initiateBridgeERC20.md
03-l1-finalizeBridgeERC20.md
```
