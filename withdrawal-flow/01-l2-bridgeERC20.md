# Function Review: L2TokenBridge.bridgeERC20(...) / bridgeERC20To(...)

## Function Code

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

Note:

```text
Original source: makerdao/op-token-bridge/src/L2TokenBridge.sol
```

## Что делает эта функция

Это user entry points для L2 -> L1 withdrawals.

`bridgeERC20(...)` withdraws на такой же address на L1.

`bridgeERC20To(...)` withdraws на выбранного recipient на L1.

Простой смысл:

```text
User starts a withdrawal from L2 to L1.
```

## Важные моменты логики

### EOA Check

```solidity
require(msg.sender.code.length == 0, "L2TokenBridge/sender-not-eoa");
```

Это делает `bridgeERC20(...)` callable только для EOAs.

### Internal Flow Call

```solidity
_initiateBridgeERC20(_localToken, _remoteToken, msg.sender, _amount, _minGasLimit, _extraData);
```

Это переводит execution в core withdrawal logic.

Burn и создание message происходят внутри `_initiateBridgeERC20(...)`.

## Main Invariants

```text
1. The withdrawal must route into the correct internal withdrawal function.
2. The intended L1 recipient must be preserved.
```

## Additional Invariants / Checks

```text
bridgeERC20(...) is intended for EOA sender -> same recipient withdrawals.
bridgeERC20To(...) is intended for custom recipient withdrawals.
```
