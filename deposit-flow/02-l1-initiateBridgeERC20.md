# Function Review: L1TokenBridge._initiateBridgeERC20(...)

## Код функции

```solidity
function _initiateBridgeERC20(
    address _localToken,
    address _remoteToken,
    address _to,
    uint256 _amount,
    uint32 _minGasLimit,
    bytes memory _extraData
) internal {
    require(isOpen == 1, "L1TokenBridge/closed");
    require(_remoteToken != address(0) && l1ToL2Token[_localToken] == _remoteToken, "L1TokenBridge/invalid-token");

    TokenLike(_localToken).transferFrom(msg.sender, escrow, _amount);

    messenger.sendMessage({
        _target: address(otherBridge),
        _message: abi.encodeCall(this.finalizeBridgeERC20, (
            _remoteToken,
            _localToken,
            msg.sender,
            _to,
            _amount,
            _extraData
        )),
        _minGasLimit: _minGasLimit
    });

    emit ERC20BridgeInitiated(_localToken, _remoteToken, msg.sender, _to, _amount, _extraData);
}
```

## Что делает

Это главная L1 deposit функция.

Она:

```text
проверяет bridge/token state
переводит токены в Escrow
создает L2 message
```

## Main Invariants

```text
1. L1 escrowed amount must equal L2 minted amount.
2. The L1 token must map to the correct L2 token.
3. The deposit message must target the correct L2 bridge.
```
