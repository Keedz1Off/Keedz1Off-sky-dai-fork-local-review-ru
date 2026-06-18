# Function Review: L2TokenBridge.finalizeBridgeERC20(...)

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
    TokenLike(_localToken).mint(_to, _amount);

    emit ERC20BridgeFinalized(_localToken, _remoteToken, _from, _to, _amount, _extraData);
}
```

## Что делает

Финализирует L1 -> L2 deposit на L2.

```text
authentic L1 message -> mint L2 tokens
```

## Main Invariants

```text
1. Only an authentic L1 -> L2 message can mint L2 tokens.
2. Minted amount must equal the L1 escrowed amount.
3. Minted token must be the correct L2 token for the L1 token.
```
