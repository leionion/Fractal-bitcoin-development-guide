## How to Mint CAT20 Token Based on CAT Protocol of Fractal Mainnet

The following are step-by-step instructions on how to mint your CAT20 token on the Fractal Mainnet. This guide provides detailed steps for setting up the necessary environment, installing required components, and executing the minting process.

**Prerequisites**

Before beginning, ensure that you have:

- A system running a supported operating system with internet connectivity.
- Docker and Docker Compose installed.
- Administrative privileges to install software packages.

### Step-by-Step Instructions

#### 1. Install Docker

Begin by installing Docker on your system to manage containerized applications.

```bash
sudo apt update && sudo apt install -y curl
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
```

#### 2. Install Docker Compose

Docker Compose is essential for defining and running multi-container Docker applications. It can be installed using the following commands:

```bash
sudo curl -L "https://github.com/docker/compose/releases/download/$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep 'tag_name' | cut -d\" -f4)/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

#### 3. Install Node.js and Yarn

Node.js and Yarn are required for managing dependencies and scripts.

- Install `npm`:

```bash
sudo apt-get install npm -y
```

- Globally install the `n` package manager and switch to a stable Node.js version:

```bash
sudo npm i n -g
sudo n stable
```

- Globally install Yarn:

```bash
sudo npm i -g yarn
```

#### 4. Clone the CAT Protocol Repository

Download the CAT Protocol repository containing all necessary code.

```bash
git clone https://github.com/CATProtocol/cat-token-box && cd cat-token-box
```

#### 5. Build and Set Up the Project

Install project dependencies and build the source code:

```bash
sudo yarn install && yarn build
```

#### 6. Run the Fractal Bitcoin Node

Navigate to the tracker directory, assign necessary permissions, and start the Fractal Bitcoin node using Docker Compose.

```bash
cd packages/tracker
sudo chmod 777 docker/data && sudo chmod 777 docker/pgdata
sudo docker compose up -d
```

#### 7. Build the Docker Image

From the project root directory, build the necessary Docker image.

```bash
cd $HOME/cat-token-box && sudo docker build -t tracker:latest .
```

#### 8. Start the Tracker Container

Run the tracker container to facilitate communication with the blockchain network.

```bash
sudo docker run -d \
    --name tracker \
    --add-host="host.docker.internal:host-gateway" \
    -e DATABASE_HOST="host.docker.internal" \
    -e RPC_HOST="host.docker.internal" \
    -p 3000:3000 \
    tracker:latest
```
If the cli command is not working well, you have to restart tracker by using this command 

```
docker restart tracker
```
#### 9. Configure Your Wallet

Navigate to the CLI directory and create a configuration file for managing wallet settings.

```bash
cd $HOME/cat-token-box/packages/cli
```

Create a `config.json` using the following template:

```bash
cat <<EOF > config.json
{
  "network": "fractal-mainnet",
  "tracker": "http://127.0.0.1:3000",
  "dataDir": ".",
  "maxFeeRate": 30,
  "rpc": {
      "url": "http://127.0.0.1:8332",
      "username": "Leionion",
      "password": "Qazxswedc"
  }
}
EOF
```

#### 10. Create or Import a Wallet

Use Yarn CLI to create a new wallet or import an existing Taproot Bitcoin wallet.

- To create a new wallet:

```bash
sudo yarn cli wallet create
```

- To import an existing Taproot wallet, modify the following command with your mnemonic phrase:

Here, Hdpath("m/86'/0'/0'/0/0") is corresponding to Unisat wallet first account's taproot address.

```bash
cat <<EOF > wallet.json
{
  "accountPath": "m/86'/0'/0'/0/0",
  "name": "leionion",
  "mnemonic": "MNEMONIC (12 words)"
}
EOF
```

Retrieve your wallet address using:

```bash
sudo yarn cli wallet address
```

#### 11. Obtain $FB CAT20 Tokens

Using this command, you can get your CAT20 token in your taproot wallet.

```bash
sudo yarn cli mint -i 1be69768c1120bd8f7477d7f1a14dc7c5b5c1c26c37306a660ad9fe472d2d36c_0 5 --fee-rate 120
```

Here, 1be69768c1120bd8f7477d7f1a14dc7c5b5c1c26c37306a660ad9fe472d2d36c_0 meaning is inscription_id(transaction_id + vout).
To mint specific token (token ID : 1be69768c1120bd8f7477d7f1a14dc7c5b5c1c26c37306a660ad9fe472d2d36c_0), you have to own minter UTXO for that token ID (1be69768c1120bd8f7477d7f1a14dc7c5b5c1c26c37306a660ad9fe472d2d36c_0)...

#### 12. Check your $FB CAT20 Token

```bash
sudo yarn cli wallet balances
```

The subsequent manual will focus on the CAT Protocol CLI Command Guide. In the [CAT_CLI_Guide](CAT_CLI_Guide.md), I will provide a comprehensive explanation on how to utilize the CAT protocol CLI commands.

If you encounter any new technical issues or need to share additional information, please contact me on Telegram at @inscNix.
