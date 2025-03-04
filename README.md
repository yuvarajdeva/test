# Arbitrum Orbit L3 Chain Setup Guide

## Introduction
This guide provides step-by-step instructions for deploying an Arbitrum Orbit Layer 3 (L3) chain. By following this document, you will successfully set up and configure your own Orbit chain using the official Orbit setup script.

## Prerequisites
Before starting, ensure you have the following:
- A machine with Linux/macOS
- Node.js & Yarn installed
- Docker installed
- An Ethereum-compatible wallet with funds
- Access to an L2 chain (e.g., Arbitrum Sepolia)

## Step 1: Launch Your Orbit Chain
1. Visit [Arbitrum Orbit](https://arbitrum.io/orbit).
2. Click **Launch a Chain**.
3. Select **Launch on Testnet**, then click **Next**.
4. Choose Chain Type (e.g., Rollup).
5. Review chain details and deploy.
6. Download the chain configuration ZIP file and proceed.

## Step 2: Setup the Orbit Chain
1. Clone the Orbit setup script repository:
   ```sh
   git clone https://github.com/OffchainLabs/orbit-setup-script.git
   cd orbit-setup-script
   ```
2. Extract the chain configuration ZIP file.
3. Move the configuration files into the `config/` directory:
   ```sh
   mv path_to_extracted_folder/nodeConfig.json config/
   mv path_to_extracted_folder/orbitSetupScriptConfig.json config/
   ```
4. Install dependencies:
   ```sh
   yarn install
   ```

## Step 3: Install Docker
Ensure that Docker is installed on your system. If not, install it using the following command:
```sh
sudo apt update
sudo apt install -y docker.io
```
Verify the installation:
```sh
docker --version
```

# Step 4: Update the Configuration

## In `docker-compose.yaml`

### Update Port Mapping:
```yaml
# Remove:
"127.0.0.1:8449:8449"

# Add:
"0.0.0.0:9090:8449"
```

### Update Configuration Path and Command:
```yaml
# Remove:
"./config:/home/user/.arbitrum"
command: --conf.file /home/user/.arbitrum/nodeConfig.json

# Add:
"./config:/home/ubuntu/.arbitrum"
command: --conf.file /home/ubuntu/.arbitrum/nodeConfig.json
```

### Update Data Directory Mapping:
```yaml
# Remove:
"./das-data:/home/user/das-data"

# Add:
"./das-data:/home/ubuntu/das-data"
```

---

## In `docker-compose/envs/common-frontend.env`

### Update API Host:
```ini
# Remove:
NEXT_PUBLIC_API_HOST=localhost

# Add:
NEXT_PUBLIC_API_HOST=10.0.1.73
```

### Update Stats API Host:
```ini
# Remove:
NEXT_PUBLIC_STATS_API_HOST=http://localhost:8080

# Add:
NEXT_PUBLIC_STATS_API_HOST=http://10.0.1.73:8080
```

### Update App Host:
```ini
# Remove:
NEXT_PUBLIC_APP_HOST=localhost

# Add:
NEXT_PUBLIC_APP_HOST=10.0.1.73
```

### Update Visualize API Host:
```ini
# Remove:
NEXT_PUBLIC_VISUALIZE_API_HOST=http://localhost:8081

# Add:
NEXT_PUBLIC_VISUALIZE_API_HOST=http://10.0.1.73:8081
```

---

## In `docker-compose/services/stats.yml`

### Add the following command:
```yaml
STATS__IGNORE_BLOCKSCOUT_API_ABSENCE=true
```

## Step 5: Start the Chain
Start the Orbit node with the following command:
```sh
docker compose up -d
```
The chain will now be running locally.

### Access BlockScout explorer at:
```
http://localhost/
```

### Your chain's RPC URL will be available at:
```
http://localhost:8449
```

## Step 5: Final Configuration & Setup
1. Fund the batch-poster and validator (staker) accounts on your underlying L2 chain.
2. Deposit ETH into your account on the chain using your chain's newly deployed bridge.
3. Deploy your Token Bridge contracts on both L2 and local Orbit chains.
4. Configure parameters on the chain.

Run the final setup command:
```sh
PRIVATE_KEY="0xYourPrivateKey" L2_RPC_URL="https://sepolia-rollup.arbitrum.io/rpc" L3_RPC_URL="http://localhost:8449" yarn run setup
```
- Replace `0xYourPrivateKey` with your actual private key.
- Ensure L2 and L3 RPC URLs are correctly set.
- After running the command, the `outputInfo.json` file will be generated, containing chain details.

### For Deposit, run the command:
```sh
PRIVATE_KEY="0xYourPrivateKey" L2_RPC_URL="https://sepolia-rollup.arbitrum.io/rpc" L3_RPC_URL="http://localhost:8449" AMOUNT="<AMOUNT>" yarn run deposit
```

## Running a Full Node for an Orbit Chain

### Minimum Hardware Configuration
To run a full node for an Orbit chain, ensure that your system meets the following minimum hardware requirements:
- **RAM:** 8-16 GB
- **CPU:** 2-4 core CPU (For AWS: t3.xLarge)
- **Storage:** Minimum 1.2 TB SSD (depends on the Orbit chain and its traffic over time)

### Installation and Setup


#### Step 1: Update Parent Chain Connection URL
Modify the connection URL to the parent chain:
```sh
--parent-chain.connection.url=https://sepolia-rollup.arbitrum.io/rpc
```

#### Step 2: Configure Child Chain Parameters
##### 1. Update `nodeConfig.json`
- Obtain `nodeConfig.json` from the L3 blockchain deployment process.
- Inside the JSON file, locate the `info-json`.
- Update the command with:
  ```sh
  --chain.info-json=<Orbit Chain's chain info>
  ```
##### 2. Set the Chain Name
The `--chain.name` flag must match the chain name used in `--chain.info-json`.
```sh
--chain.name=<My Arbitrum L3 Chain>
```
##### 3. Set Execution Forwarding Target
If running a full node (not a sequencer), set:
```sh
--execution.forwarding-target=<Your Sequencer node endpoint URL>
```
Example:
```sh
--execution.forwarding-target=http://localhost:8449
```
##### 4. Configure Sequencer Feed
Enable the node to receive the sequencer feed with:
```sh
--node.feed.input.url=<Sequencer feed URL>
```
Example:
```sh
--node.feed.input.url=http://localhost:8449
```

#### Step 3: Run the Full Node with Docker
When running the Docker image, an external volume should be mounted to persist the database across restarts. The mount point inside the Docker image should be `/home/user/.arbitrum`.

Run the following command to start the full node:
```sh
docker run --rm -it \  
  -v /some/local/dir/arbitrum:/home/user/.arbitrum \  
  -p 0.0.0.0:8547:8547 \  
  -p 0.0.0.0:8548:8548 \  
  offchainlabs/nitro-node:v3.5.1-8f247fd \  
  --parent-chain.connection.url=<Parent chain RPC URL> \  
  --chain.id=<OrbitChainId> \  
  --chain.name=<My Arbitrum Orbit Chain> \  
  --http.api=net,web3,eth \  
  --http.corsdomain=* \  
  --http.addr=0.0.0.0 \  
  --http.vhosts=* \  
  --chain.info-json=<Orbit Chain's chain info> \  
  --execution.forwarding-target=<Your Sequencer node endpoint URL>
```

---

### Congratulations! 🎉 You have successfully set up an Arbitrum Orbit L3 Chain.
