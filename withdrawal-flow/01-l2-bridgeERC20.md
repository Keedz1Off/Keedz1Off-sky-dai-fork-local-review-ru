# Function Review: L2TokenBridge.bridgeERC20(...) / bridgeERC20To(...)

## Код функции

```solidity
function bridgeERC20(
    address _localToken,
    address _remoteToken,
    uint256 _amount,
    uint32 _minGasLimit,
    bytes calldata _extraData
) external {
    require(msg.sender.code.length == 0, "L2TokenBridge/sender-not-eoa");
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

Это entry points для L2 -> L1 withdrawal.

```text
bridgeERC20(...)   = вывести самому себе на L1
bridgeERC20To(...) = вывести на custom recipient
```

## Main Invariants

```text
1. The intended L1 recipient must be preserved.
2. The custom L1 recipient must be preserved.
```
