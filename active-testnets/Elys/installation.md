
![elys](https://github.com/Core-Node-Team/Gitbook/assets/108215275/8bbc6205-2aaf-47e5-851e-c1b037faa66f)

<table data-full-width="false"><thead><tr><th align="center">Chain-ID</th><th align="center">Latest Version</th></tr></thead><tbody><tr><td align="center">elystestnet-1</td><td align="center">v0.9.0</td></tr></tbody></table>

## For automatic installation, enter this command and follow the instructions

```
curl -sSL -o elys_install.sh https://raw.githubusercontent.com/Core-Node-Team/Testnet-Guides/main/Elys/elys_install.sh && chmod +x elys_install.sh && bash ./elys_install.sh && source $HOME/.bash_profile
```


## For manuel installation follow this [guide](manuel-install.md)

## Become A Validator

### Create Keyring

* Don't forget to save the mnemonic

```bash
elysd keys add wallet
```

### Check Sync Status

* Should return _<mark style="color:green;">**False**</mark>_

```bash
elysd status 2>&1 | jq .SyncInfo
```

## Create Validator

```go
elysd tx staking create-validator \
--amount 1000000uelys \
--pubkey $(elysd tendermint show-validator) \
--moniker "MONIKER_NAME" \
--identity "KEYBASE_ID" \
--details "Core Node Community" \
--website "WEBSITE_URL" \
--chain-id elystestnet-1 \
--commission-rate 0.06 \
--commission-max-rate 0.20 \
--commission-max-change-rate 0.02 \
--min-self-delegation 1 \
--from wallet \
--gas-adjustment 1.5 \
--gas auto \
--gas-prices 1uelys \
-y
```

### Make sure you see the validator details and check

```bash
elysd q staking validator $(elysd keys show wallet --bech val -a)
```

## Yes, You Are Now A Validator On The Elys Network

### Don't Forget to save priv validator key

```bash
cat $HOME/.elys/config/priv_validator_key.json
```
