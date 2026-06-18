# Function Review: L1TokenBridge.bridgeERC20(...) / bridgeERC20To(...)

## Function Code

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

## Графический пример: <img width="811" height="303" alt="image" src="https://github.com/user-attachments/assets/54af9c15-ebc6-44b1-9fe4-fa1a7811c966" />


Note:

```text
Original source: makerdao/op-token-bridge/src/L1TokenBridge.sol
```

## Что делает эта функция

Это user entry points для L1 -> L2 deposit.

`bridgeERC20(...)` отправляет токены на такой же адрес на L2.

`bridgeERC20To(...)` отправляет токены выбранному recipient на L2.

Простой смысл:

```text
User starts a deposit from L1 to L2.
```

## Важные моменты логики

### EOA Check

```solidity
require(msg.sender.code.length == 0, "L1TokenBridge/sender-not-eoa");
```

Это делает `bridgeERC20(...)` callable только для EOA.

`bridgeERC20To(...)` не использует этот EOA check, потому что поддерживает deposits на другого recipient.

### Internal Flow Call

```solidity
_initiateBridgeERC20(_localToken, _remoteToken, msg.sender, _amount, _minGasLimit, _extraData);
```

Это переводит execution в core deposit logic.

Реальный lock и создание message происходят внутри `_initiateBridgeERC20(...)`.

## Main Invariants

```text
1. The deposit must call the correct internal function.
2. The intended recipient must be preserved.
```

## Additional Invariants / Checks

```text
bridgeERC20(...) is intended for EOA sender -> same recipient deposits.
bridgeERC20To(...) is intended for custom recipient deposits.
```
