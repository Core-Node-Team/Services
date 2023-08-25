# Installation

![babylon](https://github.com/Core-Node-Team/Gitbook/assets/108215275/fe72b6b0-bd7b-4c56-b94c-57c7c7e21eac)

<table data-full-width="false"><thead><tr><th align="center">Chain-ID</th><th align="center">Latest Version</th><th align="center">Custom Port</th></tr></thead><tbody><tr><td align="center"><mark style="color:orange;">bbn-test-2</mark></td><td align="center"><mark style="color:green;">v0.7.2</mark></td><td align="center"><mark style="color:yellow;">311</mark></td></tr></tbody></table>

> ## Hardware Requirements
<table data-header-hidden data-full-width="false"><thead><tr><th width="247">Hardware Requirements</th><th></th></tr></thead><tbody><tr><td>Minimum</td><td>3CPU 4RAM 80GB</td></tr><tr><td>Recommended</td><td>4CPU 8RAM 160GB</td></tr></tbody></table>

## For automatic installation, enter this command and follow the instructions

```bash
curl -sSL -o babylon-kurulum.sh https://raw.githubusercontent.com/Core-Node-Team/Testnet-TR/main/Babylon/babylon.sh && chmod +x babylon-kurulum.sh && bash ./babylon-kurulum.sh && source $HOME/.bash_profile
```

## For manuel installation follow this [guide](manuel-install.md)

## Become A Validator

### Create Keyring

* Don't forget to save the mnemonic

```bash
babylond keys add wallet
```

### Create BLS Key

```bash
babylond create-bls-key $(babylond keys show wallet -a)
```

* After creating a BLS key, you need to restart your node to load this key into memory.

```bash
sudo systemctl restart babylond
```

### Modify the Configuration

```bash
sed -i -e "s|^key-name *=.*|key-name = \"wallet\"|" $HOME/.babylond/config/app.toml

sed -i -e "s|^timeout_commit *=.*|timeout_commit = \"10s\"|" $HOME/.babylond/config/config.toml
```

### Get Funds

Go to Babylon [Discord](https://discord.gg/babylonglobal) and get test tokens from **#faucet** with `!faucet wallet-address`

### Check Sync Status

* Should return _<mark style="color:green;">**False**</mark>_

```bash
babylond status 2>&1 | jq .SyncInfo
```

## Create Validator

```go
babylond tx checkpointing create-validator \
--amount 1000000ubbn \
--pubkey $(babylond tendermint show-validator) \
--moniker "MONIKER_NAME" \
--identity "KEYBASE_ID" \
--details "Core Node Community" \
--website "WEBSITE_URL" \
--chain-id bbn-test-2 \
--commission-rate 0.05 \
--commission-max-rate 0.20 \
--commission-max-change-rate 0.01 \
--min-self-delegation 1 \
--from wallet \
--gas-adjustment 1.5 \
--gas auto \
--gas-prices 0.1ubbn \
-y
```

### Make sure you see the validator details and check

```bash
babylond q staking validator $(babylond keys show wallet --bech val -a)
```

## <mark style="color:purple;">Yes, You Are Now A Validator On The Babylon Network</mark>

### Don't Forget to save priv validator key

```bash
cat $HOME/.babylond/config/priv_validator_key.json
```
