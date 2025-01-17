# Welcome Genesis Validators!

We are preparing to launch the genesis process for the Side Chain mainnet. Selected validators can participate in the mainnet genesis event. The main communication point during the genesis process will be the #genesis-mainnet channel on the [SideChain Discord](https://discord.gg/sideprotocol). It is absolutely critical that you and your team join Discord during the launch period, as it will serve as the coordination point in the event of any issues during the launch process. The channel is private by default to ensure that it is free from spam and unnecessary noise.

#### The deadline for submitting a Gentx PR is Thusday, Jan 23 at 13:00 UTC 2025.

#### The pre-genesis event is broken into two parts:

-   [Part 1](/genesis_validators.md#part-1): Preparing gentx
-   [Part 2](/genesis_validators.md#part-2): Submitting gentx PR

After Gentxs are collected we will provide a genesis.json file for review. As long as there are no recommended changes we will provide the Genesis file after the collection of Gentxs.

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
git checkout v1.0.0-rc.1
cd side
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

2. Import validator key pair:
You should use the validator key or mnemonic from the testnet phase and import it into the current environment to create a genesis transaction. We have deposited appropriate Side token rewards into your validator address based on the node statistics from the testnet phase, which can be used to create a validator node for the mainnet.

```sh
// Import validator by private key:
sided keys import-hex <KEY_NAME> <HEX> --key-type segwit
```

```sh
// Import validator by mnemonics:
sided keys add <KEY_NAME> --recover --key-type segwit
```

3. Replace the genesis.json file with pre-genesis.json.

```sh
cd ~/.side/config
rm genesis.json 
wget https://github.com/sideprotocol/networks/raw/main/sidechain-1/pre-genesis.json -O genesis.json
```
Make sure your validator account is in the genesis file.

4. Create the Gentx. 
The `sided genesis gentx -h` command will provide helpful flags to configure your validator node. 

```sh
sided genesis gentx <KEY_NAME> --chain-id=sidechain-1 1000000uside
```

If all goes well, you will see a message similar to the following:

```bash
Genesis transaction written to "/home/user/.side/config/gentx/gentx-******.json"
```

# Part 2

### Submitting the Genesis transaction:

1. Rename the Gentx file to gentx-{your-moniker}.json (please do not have any spaces or special characters in the file name).

2. Fork [the networks repo](https://github.com/sideprotocol/networks/) into your GitHub account

3. Clone your repo using:

```bash
git clone https://github.com/<your-github-username>/networks
cd networks
```

4. Copy the generated gentx json file to `/sidechain-1/gentxs/`:

```bash
cd sidechain-1
cp ~/.side/config/gentx/gentx*.json ./gentx/
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
