# Eth2Airdrop
Web app to deploy a smart contract for an ERC20 token airdrop with claim links.


## Demo Video

Contract deployment and link generation:  https://screencast-o-matic.com/watch/cFQF0eqNHr   
Claiming tokens to Trust Wallet: https://youtu.be/-W2p9HgmUEQ  

## Proof-of-Concept App
You can try PoC app at https://app.eth2air.io. The app supports Ethereum Main and Ropsten Test networks right now.


## Process Overview

1. Airdropper loads the website to deploy the smart contract and generate claim links.  
2. Airdropper distributes the links to receivers in any way (e.g. special dedicated website, which sends SMS or email after submitting phone number or email)  
3. Receiver follows the link, claims the tokens.  
4. The Server calls smart contract, which verifies signatures and that link wasn’t claimed before. If everything is correct, smart-contract call distributes tokens from airdropper’s account to receiver.  


## Architecture

### Eth2air web app
 - deploys the airdrop smart-contract
 - generates claim links (as a CSV file)  
 - allows receiver to claim tokens 

The deployment and interaction with the Airdrop Smart Contract and the Relayer Server is handled by the eth2airdrop-core library - https://github.com/Eth2io/eth2airdrop-core 

### Airdrop Contract  
 - distributes tokens from Airdropper’s Ethereum Account  
 - Receiver gets tokens and ether after following a claim link  

The Smart Contract's code can be found here - https://github.com/Eth2io/eth2airdrop-core/blob/master/contracts/e2pAirEscrow.sol

### Relayer Server
An external server, which calls smart contract on behalf of the receiver. The external server pays for gas instead of receiver.

## Claim details

1. On contract deployment an Ethereum account is generated by the airdropper's browser. Address (A) is stored to the smart contract and the private key is used to sign other claim private keys.  
2. Airdropper provides to receiver a claim private key and the signature that the claim key is signed by the address A.
3. Receiver signs his Ethereum address with the claim private key and provides both signatures to the Relayer Server. 
4. The Relayer Server calls the smart contract, which verifies both signatures and that the claim private key wasn't used before. 
If everything is correct, the smart contract distributes tokens and ether to receiver's account. 


## Step-by-step guide:

If you want to deploy an airdrop:  

1. Open https://app.eth2air.io  
2. You need Metamask connected to an Ethereum address with airdropped tokens.  
3. Choose parameters of the airdrop: token address (e.g. 0xaec2e87e0a235266d9c5adc9deb4b2e29b54d009 for SNGLS), tokens dropped per link, ETH per link, amount of links    
4. Deploy the airdrop contract with a click of a button.  
5. Approve the Smart Contract to send tokens on your behalf. (Button to approve will appear after the smart contract is deployed)  
6. Download the CSV file with links generated by the web app.  
7. Distribute links to receivers.  
