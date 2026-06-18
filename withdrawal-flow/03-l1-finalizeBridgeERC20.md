# Function Review: L1TokenBridge.finalizeBridgeERC20(...)

## Код функции

```solidity
function finalizeBridgeERC20(
    address _localToken,
    address _remoteToken,
    address _from,
    address _to,
    uint256 _amount,
    bytes calldata _extraData
)
    external
    onlyOtherBridge
{
    TokenLike(_localToken).transferFrom(escrow, _to, _amount);

    emit ERC20BridgeFinalized(_localToken, _remoteToken, _from, _to, _amount, _extraData);
}
```

## Что делает

Финализирует L2 -> L1 withdrawal на L1.

```text
authentic L2 message -> release from Escrow
```

## Main Invariants

```text
1. Only an authentic L2 -> L1 message can release L1 tokens.
2. Released amount must equal the L2 burned amount.
3. Released token must be the correct L1 token for the L2 token.
```
