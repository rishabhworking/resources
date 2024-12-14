# PKRD Blockchain Installation Guide

This guide outlines the step-by-step process to install and configure the PKRD EVM-compatible blockchain on a Linux system. Follow these instructions carefully to set up your node and start interacting with the PKRD blockchain.



## Prerequisites

1. A Linux-based system with administrative privileges.
2. Basic familiarity with terminal commands.
3. Internet connection to download required files.

---

## Step 1: Install Geth

1. Navigate to your desired working directory in the terminal.

2. Download Geth version 1.13.15 from the official Ethereum website:

   ```bash
   wget https://gethstore.blob.core.windows.net/builds/geth-linux-amd64-1.13.15-c5ba367e.tar.gz
   ```

3. Extract the downloaded tarball:

   ```bash
   tar -xzvf geth-linux-amd64-1.13.15-c5ba367e.tar.gz
   ```

4. Copy the `geth` binary to a globally accessible directory:

   ```bash
   cp geth-linux-amd64-1.13.15-c5ba367e/geth /usr/local/bin/
   ```

5. Verify that Geth is installed and accessible:

   ```bash
   geth version
   ```

   You should see the Geth version details printed to the terminal.

6. Clean up unnecessary files:

   ```bash
   rm -rf geth-linux-amd64-1.13.15-c5ba367e
   rm geth-linux-amd64-1.13.15-c5ba367e.tar.gz
   ```

---

## Step 2: Create and Initialize Genesis File

1. Create a new file named `pkrd.json` in your working directory and paste the following genesis configuration into it:

   ```json
   {
     "config": {
       "chainId": 51357,
       "homesteadBlock": 0,
       "eip150Block": 0,
       "eip150Hash": "0x0000000000000000000000000000000000000000000000000000000000000000",
       "eip155Block": 0,
       "eip158Block": 0,
       "byzantiumBlock": 0,
       "constantinopleBlock": 0,
       "petersburgBlock": 0,
       "istanbulBlock": 0,
       "clique": {
         "period": 3,
         "epoch": 30000
       }
     },
     "nonce": "0x0",
     "timestamp": "0x664ee257",
     "extraData": "0x0000000000000000000000000000000000000000000000000000000000000000ddf3a5c76cda05afb0b0119f8cd3403fbfe44d2cf1ab03a61478eb34f83f6ff802a6bbff2e4984560000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
     "gasLimit": "0x47b760",
     "difficulty": "0x1",
     "mixHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
     "coinbase": "0x0000000000000000000000000000000000000000",
     "alloc": {
       "c835d379ff49f6032b8087e629c33dbb2d39aa3b": {
         "balance": "99000000000000000000000000000"
       }
     },
     "number": "0x0",
     "gasUsed": "0x0",
     "parentHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
     "baseFeePerGas": null
   }
   ```

2. Initialize the blockchain data directory with the genesis file:

   ```bash
   geth --datadir ".pkrd" init pkrd.json
   ```

   This command sets up the initial state of the blockchain based on the provided genesis file.

---

## Step 3: Run the Node

1. Start the node using the following command:

   ```bash
   screen geth --datadir ".pkrd" --networkid 51357 --port 2705 --syncmode 'full' \
     --bootnodes enode://07249f8a75743f79267a19bccccbc8c436057bd8506c157966b8a8dce3bd57596af19c1e8fe7337e6a3cf67ec4ae2b2bd6364a4af512c4ebf831b615c14c01a4@3.1.228.210:2705 \
     --gcmode=archive --ws --ws.origins="*" --ws.api "eth,net,web3,txpool" \
     --http --http.vhosts="localhost" --http.api "eth,net,web3,txpool" --http.corsdomain="*"
   ```

   - The `screen` command allows the process to run in the background.
   - The node will enable both HTTP and WebSocket communication interfaces locally.

2. Once the logs indicate that blockchain synchronization has started, minimize the session with:

   ```bash
   Ctrl + A + D
   ```

   The node will continue running in the background.

---

## JSON-RPC Communication

Your running node enables both HTTP and WebSocket communication for interacting with the blockchain. For details on JSON-RPC methods, refer to the official Ethereum documentation:

[JSON-RPC API Methods](https://ethereum.org/en/developers/docs/apis/json-rpc/)