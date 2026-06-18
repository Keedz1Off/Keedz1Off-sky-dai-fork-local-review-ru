# Function Review: L1TokenBridge.bridgeERC20(...) / bridgeERC20To(...)

## Код функции

```solidity
function bridgeERC20(
    address _localToken,
    address _remoteToken,
    uint256 _amount,
    uint32 _minGasLimit,
    bytes calldata _extraData
) external {
    require(msg.sender.code.length == 0, "L1TokenBridge/sender-not-eoa");
    _initiateBridgeERC20(_localToken, _remoteToken, msg.sender, _amount, _minGasLimit, _extraData);
}

function bridgeERC20To(
    address _localToken,
    address _remoteToken,
    address _to,
    uint256 _amount,
    uint32 _minGasLimit,
    bytes calldata _extraData
) external {
    _initiateBridgeERC20(_localToken, _remoteToken, _to, _amount, _minGasLimit, _extraData);
}
```

## Что делает

Это user entry points для L1 -> L2 deposit.

```text
bridgeERC20(...)   = отправить самому себе на L2
bridgeERC20To(...) = отправить на custom recipient
```

## Важные моменты

`bridgeERC20(...)` имеет EOA check:

```solidity
require(msg.sender.code.length == 0, "L1TokenBridge/sender-not-eoa");
```

`bridgeERC20To(...)` передает `_to` как recipient.

## Main Invariants

```text
1. The deposit must call the correct internal function.
2. The intended recipient must be preserved.
```
