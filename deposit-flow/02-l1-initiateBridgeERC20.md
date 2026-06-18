# Function Review: L1TokenBridge._initiateBridgeERC20(...)

## Function Code

```solidity
function _initiateBridgeERC20(
    address _localToken,
    address _remoteToken,
    address _to,
    uint256 _amount,
    uint32 _minGasLimit,
    bytes memory _extraData
) internal {
    require(isOpen == 1, "L1TokenBridge/closed"); // do not allow initiating new xchain messages if bridge is closed
    require(_remoteToken != address(0) && l1ToL2Token[_localToken] == _remoteToken, "L1TokenBridge/invalid-token");

    TokenLike(_localToken).transferFrom(msg.sender, escrow, _amount);

    messenger.sendMessage({
        _target: address(otherBridge),
        _message: abi.encodeCall(this.finalizeBridgeERC20, (
            // Because this call will be executed on the remote chain, we reverse the order of
            // the remote and local token addresses relative to their order in the
            // finalizeBridgeERC20 function.
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

Note:

```text
Original source: makerdao/op-token-bridge/src/L1TokenBridge.sol
```

## Что делает эта функция

Это главная L1 deposit function.

Она делает три важных действия:

```text
checks bridge/token state
locks tokens in L1 escrow
sends a message to L2
```

## Важные моменты логики

### Bridge Open Check

```solidity
require(isOpen == 1, "L1TokenBridge/closed");
```

Если bridge closed, новые deposits не могут стартовать.

### Token Mapping Check

```solidity
require(_remoteToken != address(0) && l1ToL2Token[_localToken] == _remoteToken, "L1TokenBridge/invalid-token");
```

Это проверяет, что L1 token соответствует expected L2 token.

Простой смысл:

```text
This token pair must be registered before bridging.
```

### Lock Tokens In Escrow

```solidity
TokenLike(_localToken).transferFrom(msg.sender, escrow, _amount);
```

Это переводит L1 tokens от пользователя в escrow contract.

Простой смысл:

```text
user -> escrow
```

### Create L2 Finalize Message

```solidity
_message: abi.encodeCall(this.finalizeBridgeERC20, (
    _remoteToken,
    _localToken,
    msg.sender,
    _to,
    _amount,
    _extraData
))
```

Это создает calldata для L2 bridge.

L2 bridge позже выполнит `finalizeBridgeERC20(...)` и mint токены.

### Send Message

```solidity
messenger.sendMessage({
    _target: address(otherBridge),
    _message: ...,
    _minGasLimit: _minGasLimit
});
```

Это отправляет deposit message на L2 bridge через OP messenger.

## Main Invariants

```text
1. L1 escrowed amount must equal L2 minted amount.
2. The L1 token must map to the correct L2 token.
3. The deposit message must target the correct L2 bridge.
```

## Additional Invariants / Checks

```text
The bridge must be open before sending a new message.
The recipient must be the intended recipient.
The encoded calldata must match the real L1 deposit.
```
