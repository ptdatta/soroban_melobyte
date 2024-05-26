# Melobyte
## Decentralising Music Assets using Soroban

Melobyte is a decentralised platform built on the Stellar blockchain powered by Soroban that focuses on protecting and decentralising music intellectual property. By allowing artists to upload and mint parts of their songs as royalty-bearing tokens, it ensures their creations are protected while offering fans a unique opportunity to invest directly in these musical assets. The integration with Soroban ensures fast, secure, and transparent transactions, providing an efficient solution to the challenges traditionally faced by music creators in an evolving digital landscape.

![Screenshot from 2024-05-26 23-12-07](https://github.com/ptdatta/soroban_melobyte/assets/94271200/e3f800b9-cedd-4859-a4cb-f852afeac1bd)

## Project Goal

Melobyte was inspired by drummer [Gregory Coleman](https://en.wikipedia.org/wiki/Gregory_C._Coleman), whose iconic 4-bar solo was sampled thousands of times in songs that made over millions of dollars, however he was left without compensation, resulting in him tragically dying homeless on the streets of Atlanta. 

Many artists today struggle with issues such as inadequate pay from streaming platforms, intellectual property theft, misattributed song credits, and ambiguous royalty structures. Even the most atomic parts of the song, like an individual beat, often lack protection. 

Melobyte addresses these concerns using blockchain technology. It provides artists with a platform to protect their intellectual property all the way down to the individual beats and bars and through the decentralised token system allow artists to earn royalties on their songs ensuring that they are fairly compensated, and for fans to invest in these new assets.

The following research paper also played as an inspiration: https://arxiv.org/abs/1911.08278 
# Workflow

Once the artist selects to mint their track a freighter popup appears asking the user to approve the transaction, which will invoke the host function to mint and ask to transfer 256 XLM from their account. Once the transaction has been approved and signed using the freighter wallet extension the artist is redirected to the creator uploader page. Here they input the metadata of the track, upload the full track and the album cover. They will also have the option to encrypt the upload to IPFS.

The track waveform will then be loaded and the artist annotates the track to identify sections such as Intro, Verse, Chorus etc so specific parts of the composition can be identified within the dataset. They can also view the audio fingerprint. The artist then uploads the multi-channel audio files of their song as a proof of creation to instantiate the creation medatata to a base layer that refers to the encrypted upload of the multiple channel audio files on IPFS. The self-referencing binary tree is split into the stems so at the atomic level a single beat, syllable, note or bass is a piece of intellectual property worth of protection.

Once the metadata is instantiated to the base later and the multichannel audio files are uploaded to IPFS fans and other artists can access these assets in the marketplace. An ERC721 marketplace contract is deployed to perform trust-less validation and execution of NFT trades and these tokens can be sold.


### [Rust] Smart Contracts:
- An implementation of [ERC721](https://docs.openzeppelin.com/contracts/2.x/api/token/erc721) token demonstrating how to convert an Ethereum standard to Soroban. 
- A [contract](https://github.com/ptdatta/soroban_melobyte/blob/main/erc721/src/lib.rs) that uses the converted ERC721 implementation.
- A [marketplace](https://github.com/ptdatta/soroban_melobyte/blob/main/mlh-marketplace/src/lib.rs) contract that performs trust-less validation and execution of NFT trades.

### [Bash] Deployment and Initialisation Scripts:
- A collection of Bash [scripts](https://github.com/ptdatta/soroban_melobyte/tree/main/melobyte-contract) that facilitate the deployment and initialisation of the smart contract and deployments of the local standalone network.

### [Astro] Font end implementation:
- Developed using Astro, mainly as it was a new learning experience working with this framework and is more flexible, which allowed me to build UI with any popular component library.
- Exposes the smart contract functions letting you mint, buy and transfer the tokens.

### Security
- A [storage crate](https://github.com/ptdatta/soroban_melobyte/blob/main/storage/src/lib.rs) that allows more convenient api for the Soroban storage access.
- Secure storage using [Web3Storage](https://github.com/ptdatta/soroban_melobyte/blob/0497f4a307156ec94b91a2f8afb9f894df4b9a81/melobyte-creator/App.tsx#L376) and the [CryptoJS](https://www.npmjs.com/package/crypto-js) library to encrypt uploaded files.

### [Typescript]: Components and Hooks
- These are the functions and components that make the functionality of the UI work:
  	-  A [recursive binary tree](https://github.com/ptdatta/soroban_melobyte/blob/c6901bd17ca1d7e9d8fdb75dd0e66016e2352d79/melobyte-creator/utils/binaryTreeGenerator.ts) that is used to organise the stems into their individual beats to protect the most atomic part of the song.
  	-  [Wavesurfer.js](https://github.com/ptdatta/soroban_melobyte/blob/c6901bd17ca1d7e9d8fdb75dd0e66016e2352d79/melobyte-creator/components/WaveForm/index.jsx): Used to display the waveform and the unique track fingerprintID

 ### Payments
 - Walletless Login: To allow for a familiar Google or Spotify Lgin to fasciliate the generation of ID and keys to make a streamlined process
 - Soroban - used to streamline the payment system and connect to the Stellar blockchain. Useful for scalability and ensuring instant and transparent transactions for artists and stakeholders.
 - Through the ERC721 token integration, artists can now benefit from automated token royalties.

## Demo

The video demo is: [https://www.youtube.com/watch?v=XwlzefPgSOw ](https://www.youtube.com/watch?v=-IA3In64khE)


## To launch on standalone 

### 1. Build the contracts

### 2. Run the standalone.sh script

### 3. Deploy the Contract:

```
soroban config network add standalone \
    --rpc-url http://localhost:8000/soroban/rpc \
    --network-passphrase "Standalone Network ; February 2017"
```
### 4. Create and fund an admin account:

```
soroban config identity generate admin
curl "http://localhost:8000/friendbot?addr=$(soroban config identity address admin)"
```
### 5. Deploy the init contract:

```
soroban contract deploy --wasm ./target/melobyte-init.wasm \
    --source admin 
    --network standalone
```
### 6. Invoke the initialize function with the contractID:

```
soroban contract invoke --id [CONTRACT_ID] --source admin \
    --network standalone \
    -- initialize \
	--admin admin \
	--asset $(soroban lab token id --asset native --network standalone) \
	--price 2560000000
 ```

### 7. Install the contract

### 8. Upgrade the contract with the WASM hash

### 9. Generate contract bindings

```
soroban contract bindings typescript \
    --wasm ./target/melobyte-prod.wasm \
	--network standalone \
	--contract-id [CONTRACT_ID] \
	--contract-name Melobyte \
	--output-dir node_modules/Melobyte
```
### 10. Export Web3Storage token and run:

```
export REACT_APP_WEB3_STORAGE=[TOKEN]
```


# DEMO


https://github.com/ptdatta/soroban_melobyte/assets/94271200/0df652f5-3812-455a-a385-1d02f81fc67e


# MIT license 
MIT License
Copyright (c) 2024 Parthib Datta

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so.

See the LICENSE file for details.
