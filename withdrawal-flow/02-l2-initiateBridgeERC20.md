# Function Review: L2TokenBridge._initiateBridgeERC20(...)

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
    require(isOpen == 1, "L2TokenBridge/closed");
    require(_localToken != address(0) && l1ToL2Token[_remoteToken] == _localToken, "L2TokenBridge/invalid-token");
    require(_amount <= maxWithdraws[_localToken], "L2TokenBridge/amount-too-large");

    TokenLike(_localToken).burn(msg.sender, _amount);

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

Главная L2 withdrawal функция.

Она:

```text
проверяет bridge/token state
проверяет maxWithdraw
burn L2 tokens
создает L1 release message
```

## Main Invariants

```text
1. L2 burned amount must equal L1 released amount.
2. The L2 token must map to the correct L1 token.
3. The withdrawal message must be created only after the burn step.
```
