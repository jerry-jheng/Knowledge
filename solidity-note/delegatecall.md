# DelegateCall 問題
在撰寫solidity時，我們為了不把`所有功能都寫在同一個合約中`，會選擇常用的功能切成幾個子合約作為library使用，而在使用合約呼叫這些library的功能時，會使用`delegatecall`

#### 例如合約 A 想呼叫合約 B 的方法
```
contract B {
    uint256 public number;
    function setNumber (uint256 _parameter) {
        number = _parameter;
    }
}


import "contract B.sol"
contract A {
    address public contractBAddress;
    uint256 public number;

    function delegateToContractB (bytes4 _signature, uint256 _parameter) {
        contractBAddress.delegateCall(_signature, _parameter);
    }
}
```
其中contract A 裡的 function `delegateToContractB` 是一個delegatecall的簡單例子，只要在 A 合約中 import B 合約且紀錄 B 在區塊鏈上的部署地址， 即可執行 delegatecall。

另外要注意的是 `_signature`這個參數，假設今天要在 A 合約中呼叫 B 合約的 `setNumber (uint256 _parameter)`方法， 則 _signature 要填寫的內容如下：

```
// 先以 keccak256 對 setNumber(uint256 _parameter)做 hash function 運算

web3.sha3('setNumber(uint256 _parameter)') = "0x069bfb1d171a2377c779e8d37104d3358d2ead97be4414d3fe9d8ad97515c726"

// 得到這串雜湊值後只取前八位（4個 bytes ）

_signature = "0x069bfb1d"

// 接者可以用 A 合約呼叫 B 合約 setNumber() 的方法了！

A.delegateToContractB(0x069bfb1d, 1)

// 如果以上delegatecall方法呼叫成功的話，A 合約中的 number 變數則會變成1
```

不過這邊要注意一個問題，上面的範例看似可以成功執行改變 A 合約的狀態，事實上會失敗!

原因為 solidity 的 delegatecall 中 ，假設 _signature 是 `"0x0..."` 以0開頭的話，是會失敗的，目前我還沒深入研究失敗的原因，最簡單繞過錯誤的方法就是修改delegatecall 要呼叫的方法名稱，範例如下：

```
web3.sha3('setNumber(uint256 _parameter)') = "0x069bfb1d171a2377c779e8d37104d3358d2ead97be4414d3fe9d8ad97515c726"

_signature = "0x069bfb1d"

// _signature 第一個 byte 是0，要小心！我們把 B 合約中 setNumber 方法改名為 setNum 

web3.sha3('setNum(uint256 _parameter)') = "0x6f99f349ca37f6a1952fb662d746ec82459cfaef60e2fb2a41e6cd0a57c27b54"

_signature = "0x6f99f349"

A.delegateToContractB(0x6f99f349, 1)

// 方法改名以後，這次delegatecall 合約 B 的方法就能成功把 A 合約中的 number 變數則會變成1了!
```
