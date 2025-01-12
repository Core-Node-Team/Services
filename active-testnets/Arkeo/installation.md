# Installation

![arkeo](https://github.com/Core-Node-Team/Gitbook/assets/108215275/01f3074a-af3d-4d44-a47d-800cdbbb0feb)

<table data-full-width="false"><thead><tr><th align="center">Chain-ID</th><th align="center">Latest Version</th><th align="center">Custom Port</th></tr></thead><tbody><tr><td align="center"><mark style="color:orange;">arkeo</mark></td><td align="center"><mark style="color:green;">1</mark></td><td align="center"><mark style="color:yellow;">314</mark></td></tr></tbody></table>

> ### Hardware Requirements

<table data-header-hidden data-full-width="false"><thead><tr><th width="247">Hardware Requirements</th><th></th></tr></thead><tbody><tr><td>Minimum</td><td>3CPU 4RAM 80GB</td></tr><tr><td>Recommended</td><td>4CPU 8RAM 160GB</td></tr></tbody></table>

## For automatic installation, enter this command and follow the instructions

```bash
curl -sSL -o arkeo.sh https://raw.githubusercontent.com/Core-Node-Team/scripts/main/arkeo/install.sh && chmod +x arkeo.sh && bash ./arkeo.sh && source $HOME/.bash_profile && rm arkeo.sh
```

## For manuel installation follow this [guide](manuel-install.md)

## Become A Validator

### Create Keyring

* Don't forget to save the mnemonic

```bash
arkeod keys add wallet
```

#### Get Funds

Go to Arkeo [Discord](https://discord.gg/xtfRMTfKuh) and get tokens from **#faucet** with `$request wallet-address`

#### Check Sync Status

* Should return _<mark style="color:green;">**False**</mark>_

```bash
arkeod status 2>&1 | jq .SyncInfo
```

#### Create Validator

```bash
arkeod tx staking create-validator \
--amount 1000000uarkeo \
--pubkey $(arkeod tendermint show-validator) \
--moniker "MONIKER_NAME" \
--identity "KEYBASE_ID" \
--details "Core Node Community" \
--website "WEBSITE_URL" \
--chain-id arkeo \
--commission-rate 0.1 \
--commission-max-rate 0.20 \
--commission-max-change-rate 0.03 \
--min-self-delegation 1 \
--from wallet \
--gas-adjustment 1.5 \
--gas auto \
--gas-prices 0.1uarkeo \
-y
```

#### Make sure you see the validator details and check

```bash
arkeod q staking validator $(arkeod keys show wallet --bech val -a)
```

### <mark style="color:purple;">Yes, You Are Now A Validator On The Arkeo Network</mark>

#### Don't Forget to save priv validator key

```bash
cat $HOME/.arkeo/config/priv_validator_key.json
```

### Bizi takip edin [Twitter](https://twitter.com/corenodeHQ)

### Topluluğumuza katılın [Telegram](https://t.me/corenodechat)
