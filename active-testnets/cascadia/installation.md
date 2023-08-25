# Installation

![cascadia](https://github.com/Core-Node-Team/Gitbook/assets/108215275/eeb809a7-d806-40ba-ad80-2f9e3d98ba0a)

<table data-full-width="false"><thead><tr><th align="center">Chain-ID</th><th align="center">Latest Version</th><th align="center">Custom Port</th></tr></thead><tbody><tr><td align="center"><mark style="color:orange;">cascadia_6102-1</mark></td><td align="center"><mark style="color:green;">0.1.4</mark></td><td align="center"><mark style="color:yellow;">119</mark></td></tr></tbody></table>

> ## Hardware Requirements
<table data-header-hidden data-full-width="false"><thead><tr><th width="247">Hardware Requirements</th><th> </th></tr></thead><tbody><tr><td>Minimum</td><td>4CPU 8RAM 100GB</td></tr><tr><td>Recommended</td><td>8CPU 32RAM 200GB</td></tr></tbody></table>

### For automatic installation, enter this command and follow the instructions

```bash
curl -sSl -o cascadia-kurulum.sh https://raw.githubusercontent.com/Core-Node-Team/Testnet-TR/main/Cascadia/cascadia.sh && chmod +x cascadia-kurulum.sh && bash ./cascadia-kurulum.sh
```

### For manuel installation follow this [guide](manuel-install.md)

### Become A Validator

#### Create Keyring

* Don't forget to save the mnemonic

```bash
cascadiad keys add wallet
```

#### Get Funds

Go to Cascadia [Discord](https://discord.gg/cascadia) and get test tokens from **#faucet** with `$faucet wallet-address`

#### Check Sync Status

* Should return _<mark style="color:green;">**False**</mark>_

```bash
cascadiad status 2>&1 | jq .SyncInfo
```

### Create Validator

```bash
cascadiad tx staking create-validator \
--amount 1000000aCC \
--pubkey $(cascadiad tendermint show-validator) \
--moniker "MONIKER_NAME" \
--identity "KEYBASE_ID" \
--details "Core Node Community" \
--website "WEBSITE_URL" \
--chain-id cascadia_6102-1 \
--commission-rate 0.05 \
--commission-max-rate 0.20 \
--commission-max-change-rate 0.01 \
--min-self-delegation 1 \
--from wallet \
--gas-adjustment 1.5 \
--gas auto \
--gas-prices 0.1aCC \
-y
```

#### Make sure you see the validator details and check

```sh
cascadiad q staking validator $(cascadiad keys show wallet --bech val -a)
```

### <mark style="color:purple;">Yes, You Are Now A Validator On The Cascadia Network</mark>

#### Don't Forget to save priv validator key

```bash
cat $HOME/.cascadiad/config/priv_validator_key.json
```
