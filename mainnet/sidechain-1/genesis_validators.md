# Set up a Genesis Side Chain Validator

We are excited to announce the upcoming genesis process for the Side Chain mainnet. Thank you for joining as a genesis validator. Below are instructions for submitting your GenTx and starting the network.

To ensure seamless coordination, the #genesis-validator channel on our [Side Protocol Discord](https://discord.gg/sideprotocol) will serve as the primary communication hub. The channel is private—please contact Sally (@luckysy0001) on Discord to gain access. Joining Discord during this period is crucial, as it will be the central platform for updates and issue resolution throughout the launch process.

#### The deadline for submitting your GenTx PR is Thursday, Jan 23, 2025, at 13:00 UTC.

#### The pre-genesis event is broken into two parts:

-   [Part 1](/mainnet/sidechain-1/genesis_validators.md#part-1): Preparing gentx
-   [Part 2](/mainnet/sidechain-1/genesis_validators.md#part-2): Submitting gentx PR

After collecting all GenTx submissions, we will share the genesis.json file for review. If no changes are recommended during the review period, the finalized Genesis file will be released shortly after.

**Recommended minimum hardware requirements:**

-   4 or more physical CPU cores
-   At least 500GB of SSD disk storage
-   At least 16GB of memory
-   At least 100mbps network bandwidth


# Part 1 

These instructions are for creating a basic setup of a single node. Validators should modify these instructions for their own custom setups as needed.

**Prerequisites:** Make sure to have [Golang >=1.22](https://golang.org/). You need to ensure your GOPATH configuration is correct.

### Install Side Chain:

```sh
git clone https://github.com/sideprotocol/side.git
cd side
git checkout v1.0.0-rc.1
make install
```

This will install `sided` binary into `$GOBIN`. 
You can copy the sided executable to the following destination for global use:
```sh
sudo cp ~/go/bin/sided /usr/local/bin/
```

Check that you have the right version installed:
```sh
sided version
```
Returns:
```
1.0.0-rc.1
```

### Generate genesis transaction (gentx):

1. Initialize the Side Chain directories and create a local genesis file with the correct chain-id. You will be asked to replace the temporary Genesis file with the finalized Genesis file once all participating validators submit their Gentx.

```sh
sided init <NODE_NAME> --chain-id=sidechain-1
```

2. Import Wallet:
You can create a wallet using the mnemonic from which you claimed rewards on testnets or any key with a balance greater than 1 SIDE in the `pre-genesis.json` file.

```sh
// Import validator by private key:
sided keys import-hex <KEY_NAME> <HEX> --key-type segwit
```

```sh
// Import validator by mnemonics:
sided keys add <KEY_NAME> --recover --key-type segwit
```

Note: You can replace `segwit` with `taproot` if your account uses a taproot address.

3. Replace the genesis.json file with pre-genesis.json.

```sh
cd ~/.side/config
rm genesis.json 
wget https://github.com/sideprotocol/networks/raw/main/mainnet/sidechain-1/pre-genesis.json -O genesis.json
shasum genesis.json
```
Returns:
```
04a37175ba95126f7047b057bba2df319bb78066  genesis.json
```

4. Create the Gentx. 
The `sided genesis gentx -h` command will provide helpful flags to configure your validator node. 

```sh
sided genesis gentx <KEY_NAME> 1000000uside \
  --chain-id="sidechain-1" \
  --moniker=<MONIKER> \
  --website="<webside>" \
  --details="<DETAILS>" \
  --commission-rate="0.05" \
  --commission-max-rate="0.20" \
  --commission-max-change-rate="0.01" \
  --min-self-delegation="1" \
  --identity="<IDENTITY>" \
  --pubkey="$(sided comet show-validator)"
```

<br>If your address is not found in `pre-genesis.json`, the command will fail.
<br>If successful, you will see a message similar to the following:

```bash
Genesis transaction written to "/home/user/.side/config/gentx/gentx-******.json"
```

# Part 2

### Submitting the Genesis transaction:

1. Rename the Gentx file to gentx-{your-moniker}.json (please do not have any spaces or special characters in the file name).

2. Fork [the networks repo](https://github.com/sideprotocol/networks/) into your GitHub account

3. Clone your repo using:

```bash
git clone https://github.com/<your-github-username>/networks && cd networks
```

4. Copy the generated gentx json file to `/mainnet/sidechain-1/gentxs/`:

```bash
cp ~/.side/config/gentx/gentx*.json ./mainnet/sidechain-1/gentxs/
```

5. Commit and push to your repo:

```bash
git add .
git commit -m "<your validator moniker> gentx"
git push origin main
```

6. Create a PR to https://github.com/sideprotocol/networks

For a demonstration of a step-by-step guide to creating a PR please follow the [GitHub documentation](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request-from-a-fork) or watch this helpful [youtube video](https://www.youtube.com/watch?v=a_FLqX3vGR4).

Please DM on SideProtocol's discord with a link of the GitHub PR. Only PRs from selected validators will be accepted. Validators must submit their PRs prior to the deadline submission date.

The Side Chain core team will provide Part 2 instructions for replacing the genesis.json after collecting Gentxs. Please follow on-going communication on Discord and reach out to the Side Chain core team whenever you have any questions.
