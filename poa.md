# Geth PoA 教學
- ref: [使用 go-ethereum 1.6 Clique PoA consensus 建立 Private chain](https://medium.com/taipei-ethereum-meetup/%E4%BD%BF%E7%94%A8-go-ethereum-1-6-clique-poa-consensus-%E5%BB%BA%E7%AB%8B-private-chain-1-4d359f28feff)
- ref: [IFC專案安裝與開發](http://192.168.1.88:10080/Infinitechain/guides/src/branch/master/IFC%E5%B0%88%E6%A1%88%E5%AE%89%E8%A3%9D%E8%88%87%E9%96%8B%E7%99%BC.md)

### 1. 建立創世區塊
參考 [使用 go-ethereum 1.6 Clique PoA consensus 建立 Private chain](https://medium.com/taipei-ethereum-meetup/%E4%BD%BF%E7%94%A8-go-ethereum-1-6-clique-poa-consensus-%E5%BB%BA%E7%AB%8B-private-chain-1-4d359f28feff) 生成 `poa.json`

### 2. 初始化

```
geth --datadir ./chaindata init poa.json
```

### 3. 匯入 miner keystore
參考 [IFC專案安裝與開發](http://192.168.1.88:10080/Infinitechain/guides/src/branch/master/IFC%E5%B0%88%E6%A1%88%E5%AE%89%E8%A3%9D%E8%88%87%E9%96%8B%E7%99%BC.md) 匯入 miner keystore，並將 coinbase 改為 miner address

```
geth --dev account import key.txt --datadir ./chaindata
```

### 4. 啟動 Geth
```
geth --datadir ./chaindata --rpc --rpcaddr="0.0.0.0" --rpccorsdomain="*" --rpcapi="db,eth,net,web3,personal" --unlock your_miner_address --etherbase your_miner_address
```

### 5. 連上 Geth
```
geth attach your_path/chaindata/geth.ipc
```