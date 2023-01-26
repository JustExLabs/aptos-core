---
title: "Use Aptos NFT Minting Tool"
slug: "nft-minting-tool"
---

import ThemedImage from '@theme/ThemedImage';
import useBaseUrl from '@docusaurus/useBaseUrl';

# Use the Aptos NFT Minting Tool

The Aptos Non-Fungible Token (NFT) Minting Tool allows NFT creators to upload assets, create NFT collections and run mint sites. This document explains how to launch an NFT project with the CLI and get the mint site up and running. The tool can be found in this [GitHub repo](https://github.com/aptos-labs/token).

> This tool is tested on Mac, yet it should work on Linux and Windows Subsystem for Linux (WSL) too.

## Setup

### Confirm or install Node and NPM

[Node](https://nodejs.org/en/download/) is required to run the Minting Tool CLI. If `nodejs` is not available on your operating system, make sure to install that first.

### Install Aptos CLI

[Install the Aptos CLI](../../cli-tools/aptos-cli-tool/index.md). The Aptos CLI is needed for compiling and deploying the minting contract.

### Configure Aptos account

The minting tool needs to locate your Aptos CLI profile from your user’s home folder. Run the following command after downloading Aptos CLI to make the CLI write and read config files in your user’s home folder:

```shell
aptos config set-global-config --config-type global
```

Create a new CLI profile for the NFT minting. Make sure that you choose `testnet` or `mainnet`. The `devnet` network is not supported.

If you want to reuse the existing account that you generated in [Petra Wallet](https://petra.app/docs/use) or other wallets, provide the private key of the account when prompted:

```shell
aptos init --profile nft_mint
```

Now `nft_mint` is your user profile name.

## Get the minting tool

1. Download the latest Aptos NFT Minting Tool [aptos-mint Tar gzip file](https://github.com/aptos-labs/token/releases/download/aptos-mint-v0.0.1/aptos-mint-v0.0.1.tgz).
2. Install the Aptos NFT Minting Tool with: `npm install -g <download-path>/aptos-mint-v0.0.1.tgz`
3. Verify the tool is installed correctly by running this command and receiving a version number: `aptos-mint --version`

## Create the first NFT collection

## Prepare the assets

The most popular tool for generating NFT assets is `HashLips`. You can generate NFT assets with `HashLips` or another tool of your choosing. The `aptos-mint` command works with the images and metadata generated by `HashLips` out of the box.

To make it easy to follow along, we uploaded sample assets to [hashlips_assets.zip](https://github.com/aptos-labs/token/releases/download/sample-hashlips-assets/hashlips_assets.zip).

Download the assets and decompress the compressed file.

The metadata format we need is as simple as the following:

```shell
{ "attributes": [ { "trait_type": "Background", "value": "Black" }, { "trait_type": "Eyeball", "value": "Red" } ] }
```

Note: Other fields from `HashLips` are ignored.

Each NFT should have a separate metadata file in JSON format. All NFT metadata files need to be placed within a directory (folder) named `json`. All NFT asset files need to be placed within another directory named `images`. The `json` and `images` directories need to be under the same parent directory, like so:

```
nft_assets/
├─ images/
│ ├─ 1.png
│ ├─ 2.png
│ ├─ 3.png
│ ├─ ...
├─ json/
│ ├─ 1.json
│ ├─ 2.json
│ ├─ ...
```

Each pair of image and metadata files represent an NFT. E.g. `1.png` and `1.json` in the above example are the asset and metadata files of the first NFT.

## Create an NFT project

1. Create a project called `test` with the command: `aptos-mint init --name test --asset-path <decompressed-asset-folder>`
2. Change your current directory to `test` with the command: `cd test`

## Compile and publish the smart contract

From the `test` directory, run the command: `aptos-mint publish-contract --profile nft_mint`.

## Fund the storage service

To upload assets to Arweave, we need to purchase credits. [Bundlr](https://bundlr.network/) makes the process easier. We need to fund the Bundlr service before we can upload assets.

1. Fund Bundlr with `aptos-mint fund --profile nft_mint --amount 10000000`
2. Check the balance with: `aptos-mint balance --profile nft_mint`

## Upload the assets

Once again in the `test` directory, run this command to upload assets: `aptos-mint upload --profile nft_mint`

## **Run the minting website**

Now you can run the minting site and start minting NFTs.

1. Get the minting site code from [GitHub](https://github.com/aptos-labs/token/tree/main/minting-tool/minting-site).
2. Change the current directory to the minting site folder and run: `npm install`
3. Run the following command, updating `REACT_APP_MINTING_CONTRACT` with the correct resource account address and `REACT_APP_NODE_URL` with the correct fullnode URL:

   ```shell
   REACT_APP_MINTING_CONTRACT="<resource-account-address>" REACT_APP_NODE_URL="<fullnode-network-URL>" npm run start
   ```

   The account address needs to be prefixed with `0x` and can be found in `<nft-project-folder>/config.json` by key `contractAddress`. The `REACT_APP_NODE_URL` for testnet is [`https://fullnode.testnet.aptoslabs.com`](https://fullnode.testnet.aptoslabs.com/) and for mainnet [`https://fullnode.mainnet.aptoslabs.com`](https://fullnode.testnet.aptoslabs.com/).

   Then visit the minting site at: http://localhost:3000

   Mint away!