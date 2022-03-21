# How to deploy Contract on BSC testnet

## 1.Set up Truffle
```
npm install -g truffle
```
Make an empty repository, cd into it, then
```
truffle init
```
Install HDWalletProvider
```
npm install --save truffle-hdwallet-provider
```
## 2. Create your contract
In ./contracts create a new contract called HelloWorld.sol with the following code:
```
pragma solidity ^0.4.23;
contract HelloWorld {
    function sayHello() public pure returns(string){
        return(“hello world”);
    }
}
```
## 3. Deploy your contract
In ./migrations, create a deployment script specifically named 2_deploy_contracts.js with the following code:
```
var HelloWorld = artifacts.require(“HelloWorld”);
module.exports = function(deployer) {
    deployer.deploy(HelloWorld, “hello”);
    // Additional contracts can be deployed here
};
```
## 4. Configure BSC testnet network and the provider
```
const HDWalletProvider = require('@truffle/hdwallet-provider');
const path = require("path");
require('dotenv').config()

module.exports = {
  // See <http://truffleframework.com/docs/advanced/configuration>
  // to customize your Truffle configuration!
  contracts_build_directory: path.join(__dirname, "client/src/contracts"),
  networks: {
    develop: {
      port: 8545
    },
    testnet: {
      provider: () => new HDWalletProvider(process.env.MNEMONIC, `https://data-seed-prebsc-1-s1.binance.org:8545`),
      network_id: 97,
      confirmations: 10,
      timeoutBlocks: 200,
      skipDryRun: true
    },
    bsc: {
      provider: () => new HDWalletProvider(process.env.MNEMONIC, `https://bsc-dataseed1.binance.org`),
      network_id: 56,
      confirmations: 10,
      timeoutBlocks: 200,
      skipDryRun: true
    },
  }
};
```
```
truffle deploy --network ropsten
```
## 5. Access your deployed contract
```
truffle console --network testnet
```
## 6. Finally, invoke contract function and say hello!
```
HelloWorld.deployed().then(function(instance){return instance.sayHello()});Also, Read
```
