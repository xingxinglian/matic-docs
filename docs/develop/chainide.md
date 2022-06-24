---
id: chainide
title: Using ChainIDE
sidebar_label: Using ChainIDE
description: Learn how to use ChainIDE to code, build, and deploy smart contracts on Polygon.
keywords:
  - docs
  - matic
  - polygon
  - IDE
  - build
  - deploy
  - environment
  - smart contract
  - chainIDE
image: https://matic.network/banners/matic-network-16x9.png 
---

## Introduction

[ChainIDE](https://chainide.com/) is a chain agnostic, cloud-based IDE for creating decentralized applications. It enhances development cycle through pre-configured plugins that save users' time and effort. This is a beginner guide on creating and deploying a simple ERC-721 smart contract on the Polygon Mumbai Testnet. If you have any questions, feel free to ask them in the [ChainIDE Discord](https://discord.gg/QpGq4hjWrh).

## Pre-requisites

1. ChainIDE
2. MetaMask
3. Solidity

## What You'll Do

The following are general steps for deploying an ERC-721 smart contract:

1.  Setting up a wallet
2.  Write down an ERC-721 smart contract
3.  Compile an ERC-721 Smart Contract
4.  Deploy an ERC-721 Smart Contract
5.  Create a Flattened File using Flattener Library
6.  Verify a Smart Contract
7.  NFT Minting

## Setting up a Wallet

### Install MetaMask

When deploying a smart contract on the blockchain or when making a transaction to a deployed smart contract, a gas fee must be paid, and for that, we need to use a crypto wallet which can be MetaMask. Click [here](https://metamask.io/) to install MetaMask. 

###  Manually adding the Mumbai testnet

After installing MetaMask, you need to manually add the Polygon Mumbai Testnet to MetaMask. To add Polygon Mumbai Testnet to MetaMask, see the [MetaMask Polygon documentation](https://docs.polygon.technology/docs/develop/metamask/config-polygon-on-metamask#add-the-polygon-network-manually). 

<div style={{ display: 'flex', justifyContent: 'center', alignItems: 'center' }}>
  <img src="https://chainide-doc.s3.amazonaws.com/ERC+721+Deployment+on++Mumbai/image.png" />
</div>

### Obtaining Testnet Matic

Once Mumbai has been added to MetaMask, navigate to the [Polygon Faucet](https://faucet.polygon.technology/) to receive test tokens. Tokens are needed to pay for gas fees to deploy and interact with the smart contract. On the faucet page, choose Mumbai as the network, MATIC as the token and paste your MetaMask wallet address. Then, click submit and faucet will send you some test MATIC within a minute.

<img src="https://chainide-doc.s3.amazonaws.com/ERC+721+Deployment+on++Mumbai/Polygon_PR_get_tokens.png" width="100%" height="100%" />

## Write an ERC-721 Smart Contract

You need to write down all the required functions that you want to implement in your ERC-721 smart contract. A general ERC-721 smart contract has the following functions:

-   `balanceOf()`: return by_ The number of NFTs held by the owner
    
-   `ownerOf()`: returns the address of the token holder
    
-   `approve():`grant address_ To has_ Token ID control. Approval event needs to be triggered after the method is successful
    
-   `setApprovalForAll()`: Grant address_ The operator has the control of all NFTs, and the approvalforall event needs to be triggered after success
    
-   `getApproved()` : Get the approved address for a single NFT
    
-   `isApprovedForAll()`: Query if an address is an authorized operator for another address
    
-   `safeTransferFrom()`: To transfer the ownership of an NFT, a successful transfer operation must initiate a transfer event
    
-   `transferFrom()`: Used to transfer NFTs. After the method succeeds, it needs to trigger the transfer event. The caller confirms himself_ To address can receive NFT normally, otherwise, this NFT will be lost. When this function is implemented, it needs to check whether it meets the judgment conditions

The ChainIDE team has prepared a complete ERC-721 template contract that includes all the required functions; you may use this built-in template and add/delete functions according to your requirements.

Visit the [ChainIDE site](www.chainide.com) and click on "Try Now".

<img src="https://3869740696-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-MYy-lqJKjq1m0yBAX4r%2Fuploads%2Fnpdf7fg51675wYmFcL6b%2Fimage.png?alt=media&token=353fc876-a319-49cb-92d5-1ed23c39aa90" width="100%" height="100%" />

Then, click on "New Project" and select "Polygon" , "ERC721 Showcase".

<img src="https://chainide-doc.s3.amazonaws.com/Using+ChainIDE+polygon/select+polygon+showcase.png" width="100%" height="100%" />

Now, you can see the template contract, **Creature.sol**, that includes all the required functions.

After creating the project, click on the "unconnected button" in the upper right corner, select the "Injected Web3 Provider" button, and then select on MetaMask to connect to the MetaMask wallet (Polygon Mainnet is the main network, and Mumbai is the test network - connect to Mumbai).

<img src="https://chainide-doc.s3.amazonaws.com/Using+ChainIDE+polygon/connect+mumbai.png" width="100%" height="100%" />

## Compile an ERC-721 Smart Contract

After you have completed your smart contract, it is time to compile it. To compile, navigate to "compile the module", choose an appropriate compiler according to your source code, and press the "compile" button. An ABI and bytecode for the source code generate upon successful compilation. If there are some errors in your source code, they will display under the output panel in the "Logger module". You may need to carefully read the error, resolve it accordingly and compile the contract again.  

::: note

Note down the compiler version and the license for your source code as it would be needed when you verify your smart contract on the Polygon Mumbai Testnet.

:::

<img src="https://chainide-doc.s3.amazonaws.com/ERC+721+Deployment+on++Mumbai/image+(1).png" width="100%" height="100%" />

## Deploy an ERC-721 Smart Contract

After successful compilation, it's time to deploy your compiled ERC-721 smart contract to the Polygon Mumbai test network. For that, you need to have a MetaMask installed, the Polygon Mumbai test network added to your wallet, and some testnet tokens to pay for the transaction fee.

Navigate to the "Deploy & Interaction" module and choose among the compiled smart contract. Select the smart contract you want to deploy and click the "deploy" button. For this tutorial, the `GameItem` smart contract will be deployed.

<img src="https://chainide-doc.s3.amazonaws.com/ERC+721+Deployment+on++Mumbai/image+(2).png" width="100%" height="100%" />

After successful deployment, an output message should state that your smart contract was deployed successfully. You can now verify the deployed contract. All the functions in the deployed smart contract can be seen in the "INTERACT" panel.

<img src="https://chainide-doc.s3.amazonaws.com/ERC+721+Deployment+on++Mumbai/image+(3).png" width="100%" height="100%" />

## Create a Flattened File using Flattener Library

To verify a smart contract that imports other smart contracts, we need to create a flattened file, a flattened file including all the source code of imported contracts in a single file. To create a flattened file, you need to add a "Flattener" plug-in.

<img src="https://chainide-doc.s3.amazonaws.com/ERC+721+Deployment+on++Mumbai/image+(7).png" width="100%" height="100%" />

Once the Flatterner plug-in is activated, you'll be able to access it as a separate module as shown in the figure below. Choose the compiled file, and click on the flatten button to create a flattened file, once the flattened file is created, it will be automatically copied to the clipboard, you may paste it to a file and save it for later usage.

<img src="https://chainide-doc.s3.amazonaws.com/ERC+721+Deployment+on++Mumbai/image+(8).png" width="100%" height="100%" />

If you want to save the flattened file, click save button, and a flattened file will be saved in the current repository.

<img src="https://chainide-doc.s3.amazonaws.com/ERC+721+Deployment+on++Mumbai/image+(9).png" width="100%" height="100%" />

The saved flattened file can be access under the explorer module.

<img src="https://chainide-doc.s3.amazonaws.com/ERC+721+Deployment+on++Mumbai/image+(10).png" width="100%" height="100%" />

## Verify a Smart Contract

To verify a smart contract, you need to visit [Mumbai Polygonscan](https://mumbai.polygonscan.com/), and search for the deployed smart contract using the contract address.

<img src="https://chainide-doc.s3.amazonaws.com/ERC+721+Deployment+on++Mumbai/image+(11).png" width="100%" height="100%" />

Click on the "verify and publish" link shown under the contract section.

<img src="https://chainide-doc.s3.amazonaws.com/ERC+721+Deployment+on++Mumbai/image+(12).png" width="100%" height="100%" />

Once you click on the verify and publish link, you will be asked for the following:

-   Contract Address: The address of a deployed smart contract that you want to verify
    
-   Compiler Type: Either you want to verify a single file or multiple files
    
-   Compiler Version: The compiler version that you used to compile the smart contract
    
-   License: Open-source license type that you used for your source code
    

<img src="https://chainide-doc.s3.amazonaws.com/ERC+721+Deployment+on++Mumbai/image+(13).png" width="100%" height="100%" />

After that, you need to paste the flattened file that you created in step 5, and your smart contract will be verified.

<img src="https://chainide-doc.s3.amazonaws.com/ERC+721+Deployment+on++Mumbai/image+(14).png" width="100%" height="100%" />

If there are no issues with your smart contract, it would be verified, and you'll be able to see an image similar to the one that is shown below.

<img src="https://chainide-doc.s3.amazonaws.com/ERC+721+Deployment+on++Mumbai/image+(15).png" width="100%" height="100%" />

## NFT Minting

To mint an NFT, you need to use the "award item" function, the wallet address of someone to whom you want to award an NFT too, and the link of the photo uploaded to IPFS needs to be pasted in the token URL input field.

<img src="https://chainide-doc.s3.amazonaws.com/ERC+721+Deployment+on++Mumbai/image+(4).png" width="100%" height="100%" />

<img src="https://chainide-doc.s3.amazonaws.com/ERC+721+Deployment+on++Mumbai/image+(5).png" width="100%" height="100%" />

After successful minting, you can check the minted NFT on the OpenSea NFT marketplace. Visit [OpenSea Testnet](https://testnets.opensea.io/), connect your MetaMask wallet and make sure the selected network is Polygon Mumbai Testnet, and you'll be able to see and trade the minted NFT on the OpenSea NFT marketplace.

<img src="https://chainide-doc.s3.amazonaws.com/ERC+721+Deployment+on++Mumbai/image+(6).png" width="100%" height="100%" />

Congratulations, you have successfully minted an NFT on Polygon using ChainIDE.