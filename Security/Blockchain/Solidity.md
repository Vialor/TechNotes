# Types
**Value Types:**
they are always copied when they are used as function arguments or in assignments
```Solidity
uint(uint256), int(int256), bool, address, bytes(bytes32)
contract, function, enum
```
**Reference Types:**
```Solidity
struct, array, mapping
```
**State Variable Visibility:**
public: same as internal, but compiler automatically generates getter functions for them
internal(this is default): accessible from within the contract they are defined in and in derived contracts
private: same as internal, but not visible in derived contracts
**Function Visibility**
external: part of the contract interface, which means they can be called from other contracts and via transactions. An external function `f` cannot be called internally (i.e. `f()` does not work, but `this.f()` works)
public: same as external, but can be called internally
internal: accessible from within the contract they are defined in and in derived contracts
private: same as internal, but not visible in derived contracts
  
**Notes:**
1. No gas for calling **view, pure** function, but they do cost has when being called by a gas calling function.
2. Making something `private` or `internal` only prevents other contracts from reading or modifying the information, but it will still be visible to the whole world outside of the blockchain.
3. Use `constatant` (`CAPTITALIZED_VARIABLE`) and `immutable` (`i_variable`) to save gas.
  
# EVM
Ethereum Virtual Machine
EVM can interact data in 6 places: stack, **memory**, **storage**, **calldata**, code, logs
**storage**: the location where the state variables are stored, where the lifetime is limited to the lifetime of a contract
**memory**: lifetime is limited to an external function call
**calldata**: a **non-modifiable**, non-persistent area where function arguments are stored, and behaves mostly like memory.
  
# Globals
block
msg
sender,value
tx
  
# Sending ETH
```Solidity
payable(msg.sender).transfer(address(this).balance)
require(payable(msg.sender).transfer(address(this).balance), "Send Failed")
(bool callSuccess,) = payable(msg.sender).call{value: address(this).balance}("")
require(callSuccess, "Call failed")
```
  
# Special Functions
```Solidity
fallback() external payable
receive() external payable
construct()
```