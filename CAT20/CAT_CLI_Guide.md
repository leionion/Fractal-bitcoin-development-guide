## Ultimate Guide on Using the CAT CLI Command

The CAT CLI provides a comprehensive interface for interacting with CATs (Chia Asset Tokens). Below is a step-by-step guide to using the CAT command line interface effectively:

#### Step 1: Configuration

Begin by copying the example configuration file to your working configuration file. This step ensures that your CLI commands will use the correct settings.

- **Copy Configuration File**: Duplicate the provided `config.example.json` file and rename it to `config.json`.
- **Update Configuration**: Modify `config.json` with your specific configuration settings as needed for your environment.

The CLI commands will default to using `config.json` in the current directory. However, if you prefer to use a different configuration file, specify it using the `--config=your.json` option.

#### Step 2: Creating a Wallet

To create a new wallet, execute the following command:

```bash
yarn cli wallet create
```

Upon execution, you should receive an output similar to the following:

```
What is the mnemonic value of your account? (default: generate a new mnemonic) ********
Your wallet mnemonic is:  ********
exporting address to the RPC node ...
successfully.
```

This output indicates that a mnemonic for your wallet has been generated, which is crucial for recovering your wallet if necessary. Ensure to save this mnemonic in a secure location.

#### Step 3: Displaying Your Address

To show the address of your wallet, use the following command:

```bash
yarn cli wallet address
```

The expected output will be similar to:

```
Your address is bc1plfkwa8r7tt8vvtwu0wgy2m70d6cs7gwswtls0shpv9vn6h4qe7gqjjjf86
```

This address is where you'll receive funds and interact with specific CATs.

#### Step 4: Funding Your Address

To perform transactions using your wallet, deposit a certain amount of satoshis to the displayed address. This step is essential for enabling further interactions with the network.

#### Step 5: Displaying Token Balances

You can check the balances of your tokens within the wallet using the following command:

```bash
yarn cli wallet balances
```

The output should look like this:

```
┌──────────────────────────────────────────────────────────────────────┬────────┬─────────┐
│ tokenId                                                              │ symbol │ balance │
┼──────────────────────────────────────────────────────────────────────┼────────┼─────────┤
│ '45ee725c2c5993b3e4d308842d87e973bf1951f5f7a804b21e4dd964ecd12d6b_0' │ 'CAT'  │ '18.91' │
┴──────────────────────────────────────────────────────────────────────┴────────┴─────────┘
```

This tabular representation provides a clear view of each token's ID, symbol, and the respective balance available in your wallet.

#### 6. Deploy Token

To deploy a token within the CAT protocol, you have the option to utilize a metadata JSON file or specify parameters directly through command line options.

##### Deploy Using Metadata JSON:

To deploy a token with pre-defined metadata, include the JSON file specifying the necessary details:

```bash
yarn cli deploy --metadata=example.json
```

Below is an example of the `example.json` file:

```json
{
  "name": "cat",
  "symbol": "CAT",
  "decimals": 2,
  "max": "21000000",
  "limit": "5",
  "premine": "0"
}
```

##### Deploy With Command Line Options:

Alternatively, deploy your token by specifying attributes directly in the command line:

```bash
yarn cli deploy --name=cat --symbol=CAT --decimals=2 --max=21000000 --premine=0 --limit=5
```

**Expected Output:**
Upon successful deployment, you will receive confirmation similar to the following:

```
Token CAT has been deployed.
TokenId: 45ee725c2c5993b3e4d308842d87e973bf1951f5f7a804b21e4dd964ecd12d6b_0
Genesis txid: 45ee725c2c5993b3e4d308842d87e973bf1951f5f7a804b21e4dd964ecd12d6b
Reveal txid: 9a3fcb5a8344f53f2ba580f7d488469346bff9efe7780fbbf8d3490e3a3a0cd7
```

**Important:** Ensure that the product of `max` and 10 raised to the power of `decimals` is less than 2^31, due to constraints of Bitcoin Script which supports only 32-bit signed integers. Efforts are underway to extend this support to 64-bit or larger integers.

#### 7. Mint Token

The minting process allows for the creation of new tokens. If the `amount` parameter is not specified, the default is to mint the number of tokens specified by `limit`.

```bash
yarn cli mint -i [tokenId] [amount?]
```

**Expected Output:**
Upon successful minting, you will see a confirmation like:

```
Minting 5.00 CAT tokens in txid: 4659529141de4996ad8482910ef3e0cf63665c39e62b86f17d5d398b5748b66b ...
```

**Note:** Due to UTXO contention, there is a possibility of a minting failure if another user attempts to mint simultaneously with the same minter UTXO. In case of failure, retry until success is achieved. For more details, refer to [UTXO contention](https://catprotocol.org/cat20#parallel-mint).

#### 8. Send Token

Sending tokens to another address is straightforward. Specify the tokenId, receiver's address, and the amount to send.

```bash
yarn cli send -i [tokenId] [receiver] [amount]
```

**Expected Output:**
Upon successful sending, you will receive an output along the lines of:

```
Sending 1.11 CAT tokens to bc1pmc274s6lalf6afrll2e23m2qmk50dwaj6srjupe5vyu4dcy66zyss2r3dy
in txid: 94e3254c1237ba7cd42eaeeae713c646ee5dd1cd6c4dd6ef07241d5336cd2aa7
```

### Fee Rate

For the `deploy`, `mint`, and `send` commands, you can specify a fee rate by using the `--fee-rate` option to customize the transaction fees according to network conditions.
