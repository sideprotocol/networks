# sidechain-1

## Get Started

### Prerequisite‌

1. Install Golang

   > Note: 1.22.0+ is required.

Refer to the [official install documentation](https://golang.google.cn/doc/install).

### Cloning SIDE Repository and Setup

1. Clone the SIDE repository:

   > NOTE: Please remove previous git working directory (side) on your local and re-clone the repository.

   ```sh
   git clone https://github.com/sideprotocol/side.git
   ```

2. Move to the SIDE directory:

   ```sh
   cd side
   ```

3. Checkout to the desired version:

   ```sh
   git checkout v1.0.0
   ```

4. Install SIDE:

   ```sh
   make install
   ```

5. Verify installation:

   ```sh
   sided version --long
   ```

   ```sh
   commit: 447f80114d1099cb52b77d6e0e30995f63f5a6ab
   cosmos_sdk_version: v0.50.9-btc1
   go: go version go1.22.3 darwin/arm64
   name: sidechain
   server_name: sided
   version: 1.0.0
   ```

> Note: The commit for `v1.0.0` is `447f80114d1099cb52b77d6e0e30995f63f5a6ab`

### Launch

1. Download the genesis file:

```sh
wget https://github.com/sideprotocol/networks/raw/main/mainnet/sidechain-1/genesis.json -O ~/.side/config/genesis.json
```

2. Verify the SHA256 hash of the downloaded genesis file:

```sh
shasum -a 256 ~/.side/config/genesis.json
```

Expected output:
```
469bb84bbd261c4d39c816cbea6966c6169804629cf3a4d177f66f48818ea75b  genesis.json
```

3. Set up the persistent peer:

Set `persistent_peers` in `<home>/config/app.toml` to the following peer:

```sh
0ca21af519767961a10a9b96a10ebcbc8ab7b5e6@209.250.232.135:26656
```

4. Start node:

```sh
sided start
```

> Note: For validators, `minimum-gas-prices` is required and `0.0006uside,0.000001sat` is recommended. You can set it in `<home>/config/app.toml` or start with the flag:

```sh
sided start --minimum-gas-prices=0.0006uside,0.000001sat
```

### Validating

> Note: The step is not required for genesis validator.

1. Create the validator address

```sh
sided keys add <key name> --key-type=<key type>
```

> Note: The key type can be `Segwit` or `Taproot`.

2. Create Validator

> Note: Please fund the above generated address first.

Place the validator info to a JSON file:

```sh
{
        "pubkey": {"@type":"/cosmos.crypto.ed25519.PubKey","key":"<PUBLIC KEY>"},
        "amount": "<AMOUNT>",
        "moniker": "<MONIKER>",
        "identity": "optional identity signature (ex. UPort or Keybase)",
        "website": "validator's (optional) website",
        "security": "validator's (optional) security contact email",
        "details": "validator's (optional) details",
        "commission-rate": "0.1",
        "commission-max-rate": "0.2",
        "commission-max-change-rate": "0.01",
        "min-self-delegation": "1"
}
```

> Note: The public key can be obtained via the following command:

```sh
sided comet show-validator
```

Create validator with the specified path:

```sh
sided tx staking create-validator <path/to/validator.json> --from <KEY_NAME> --chain-id sidechain-1 --fees 1000uside
```
