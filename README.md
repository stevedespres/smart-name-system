
#  Smart Name System

## Description
A decentralized application built with Solidity for Ethereum Blockchain. 
It provides a name registry and some services.
This application has been built as final project during the [Consensys Developer Bootcamp](https://consensys.net/academy/bootcamp/)
It's composed of several smart contracts and a web application.

Demo:

[![IMAGE ALT TEXT HERE](https://zupimages.net/up/20/02/2xoy.png)](https://www.youtube.com/watch?v=alRL7uzalAE)`

Smart contracts deployed on Rinkeby testnet : 
- SmartNameRegistry - [0x0F9A836E09BEBe5AB18A4B4b0C1F8A5de2C63516](https://rinkeby.etherscan.io/address/0x0F9A836E09BEBe5AB18A4B4b0C1F8A5de2C63516)
- SmartNameLibrary  - [0x3667a4592c37FeA78fdEb5C6981B9E85dFB4D2c7](https://rinkeby.etherscan.io/address/0x3667a4592c37FeA78fdEb5C6981B9E85dFB4D2c7)
- SmartNameResolver - [0x4a639749D4d8f519C9Aad77cD6382D0AC2f4772A](https://rinkeby.etherscan.io/address/0x4a639749D4d8f519C9Aad77cD6382D0AC2f4772A)
- SmartNameMarket   - [0xf274dFe3A60D4cCDD91439cC8681272392253C6d](https://rinkeby.etherscan.io/address/0xf274dFe3A60D4cCDD91439cC8681272392253C6d)
- SmartNameBanking  - [0x19A2Dd0C0385bBA869feef3981750eb80a66BAd1](https://rinkeby.etherscan.io/address/0x19A2Dd0C0385bBA869feef3981750eb80a66BAd1)


## Features

- Register and manage names with the same format as tradtional domain names (ex: mywebsite.org)
- Associate Ethereum public address to a name, to not memorize a complex address (ex: 0x137DCF94E2DEFF923e0A609b1d877BE9288243C8 )
- Send crypto currencies to a name
- Buy / Sell names on a marketplace
- Resolve names

This application generates a smart contract for each name registered. That allows to represent a name by an asset, to certify it and create a proof of existence on Blockchain. Moreover, the name can be exchanged without third party service. 

## Concepts

### SmartName

A **SmartName** is a name with the same format as a traditional domain name `name.tld` (test.com, steve.fr, etc.).
The `name` is limited to 16 characters and the `tld` to 4 characters. They are no restrictions on the content of the `name` and `tld`  unlike the registers of traditional domain names.
A Smart Name is composed of : 

 - `id` (bytes32) : unique identifier
 - `name` (bytes16) : name of the SmartName
 - `ext` (bytes4) : extension (tld) of the SmartName
 - `administrator` (address) : address of the owner
 - `record` (address) : address associated with the SmartName

### Registry

A **SmartNameRegistry** allows to manage the SmartNames. Users can use the registry to : 
 - **Register** a SmartName
 - **Abandon** a SmartName
 - **Modify the ownership** of a SmartName
 - **Modify the record associated** with a SmartName
 - **Get informations** about SmartNames registered

The implementation of the registry implements some design patterns to manage access, to control contract life cycle and to provide security.

 ### Services
To create services and provide some applications with these SmartNames, a **SmartNameService** contract can be used. It provides an interface that allows to communicate with the SmartNameRegistry. 

 ### Resolver
**SmartNameResolver** is a service that allows to get some informations about a SmartName like : 

 - Which is the **administrator**
 - What is the **record** associated 
 - What is the **address** of the SmartName contract
 
 ### Banking
**SmartNameBanking** is a service that allows to send Ethers to a SmartName. Amount of Ethers is sent to the record address associated with the SmartName. 

 ### Market
**SmartNameMarket** is a marketplace that allows to put on sale and buy SmartNames.


## Possible improvements

Several things can be improved : 


- SmartName as **non-fungible token**
	- SmartNames are unique object, with their own properties. So, they can be represented with a standard non-fungible token like [ERC-721](http://erc721.org/). 

- Bridge between **smart names** and **domain names** to **protect identity**
	- A no restrictions registry allows anybody to register any names ; it's can be problematic like with the domain names, with [phishing](https://en.wikipedia.org/wiki/Phishing) and [cybersquatting](https://en.wikipedia.org/wiki/Cybersquatting)
	- To prevent that, we can **restrict the register** of a smart name only to the **real owner of the same domain name**. For example, if Bob wants to register the smart name bob.com, he should register the domain name bob.com with a registrar (Cloudflare, OVH, etc.). These restrictions can be applied only with existent extensions.
	- To do that, it's possible to certify the ownership of the domain name with DNSSEC and add a TXT record with an Ethereum public address. During the register of the smart name, the contract can verify that the Ethereum address of the user matches with the TXT record of the domain name. This system is already used by Ethereum Name Service and an oracle exists : https://github.com/ensdomains/dnssec-oracle
- **Re-organize** registry contract
	- Actually, one contract can only support this format : `name.tld`. To manage subdomains (`sub.name.tld`) and better organize the data, I think it will be better to take example of a traditional system of domain names. 
![System of Domain Names](https://zupimages.net/up/20/02/jsz4.png
)
	- Each level can be represented by a smart contract
- Fix some **bugs**
- Implement **others design patterns** to manage roles, store list of names, optimize gas, etc. 
- Audit **security** 

# Technical description

## Technologies
### Contracts 

 - Solidity 
 - Truffle 
 - Javascript tests with web3

### Web application

 - VueJS app
 - Web3

## Architecture

### Simple UML 
![Simple UML](https://zupimages.net/up/20/02/wxpe.png)
### Complete UML

![Complete UML](https://zupimages.net/up/20/02/qe1d.png) 
## Design patterns and security

### [Design Pattern decisions](https://github.com/stevedespres/smart-name-system/blob/master/design_pattern_decisions.md)
### [Avoiding common attacks](https://github.com/stevedespres/smart-name-system/blob/master/avoiding_common_attacks.md)
# Test it

## Prerequisites
### Installation

 - Install [NodeJS](https://nodejs.org/fr/download/)
 - Install [Truffle](https://www.trufflesuite.com/docs/truffle/getting-started/installation)
 - Install [GanacheCLI](https://github.com/trufflesuite/ganache-cli)
 - Install [VueCLI](https://cli.vuejs.org/guide/installation.html)
 - Install [Metamask](https://metamask.io/) on web browser
 - Clone project
 - In `smart-name-system-app` and `smart-name-system-contracts` : `npm install` 

## Tests

### Launch private blockchain with Ganache

    ganache-cli
### Launch tests
    cd smart-name-system-contracts

    truffle tests
    
## Application

### Launch private blockchain with Ganache

    ganache-cli
   Copy private key of an account from the ganache-cli terminal.
    
### Deploy contracts 
    cd smart-name-system-contracts
    truffle migrate
If error of type `not found compiled contract`, delete `/build/contract` folder

### Start application
    cd smart-name-system-app
    npm run serve
### Connect to Metamask
 1. Open web brower 
 2. Create Metamask wallet 
 3. Import account 
 4. Paste private key of account 
 5. Connect with this account

### Go to application

Go to `localhost:8080`and enjoy



