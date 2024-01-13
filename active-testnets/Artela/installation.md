# Installation
![artela](https://github.com/molla202/Artela/assets/91562185/a7922117-442e-4bbf-b56a-1d11e09670f7)

<table data-full-width="false"><thead><tr><th align="center">Chain-ID</th><th align="center">Latest Version</th><th align="center">Custom Port</th></tr></thead><tbody><tr><td align="center"><mark style="color:orange;">artela_11822-1</mark></td><td align="center"><mark style="color:green;">v0.4.7-rc4</mark></td><td align="center"><mark style="color:yellow;">317</mark></td></tr></tbody></table>


> ## Hardware Requirements
<table data-header-hidden data-full-width="false"><thead><tr><th width="247">Hardware Requirements</th><th></th></tr></thead><tbody><tr><td>Minimum</td><td>3CPU 4RAM 80GB</td></tr><tr><td>Recommended</td><td>4CPU 8RAM 160GB</td></tr></tbody></table>

## For automatic installation, enter this command and follow the instructions
```bash
curl -sSL -o artela.sh https://raw.githubusercontent.com/Core-Node-Team/scripts/main/artela/install.sh && chmod +x arkeo.sh && bash ./artela.sh && source $HOME/.bash_profile && rm artela.sh
```
## For manuel installation follow this [guide](manuel-install.md)

## Become A Validator

### Create Keyring

* Don't forget to save the mnemonic

```bash
artelad keys add wallet
```
#### Get Funds

Go to Arkeo [Discord](https://discord.gg/TzmnmuCU) and get tokens from **#faucet** with `$request wallet-address`

#### Check Sync Status

* Should return _<mark style="color:green;">**False**</mark>_

```bash
artelad status 2>&1 | jq .SyncInfo
```

#### Create Validator

```bash
artelad tx staking create-validator \
--amount 950000uart \
--from cüzdan-adınız \
--commission-rate 0.1 \
--commission-max-rate 0.2 \
--commission-max-change-rate 0.01 \
--min-self-delegation 1 \
--pubkey $(artelad tendermint show-validator) \
--moniker "moniker-adınız" \
--identity="" \
--website="" \
--details="Mustafa Kemal ATATÜRK❤️" \
--chain-id artela_11822-1 \
--gas auto \
--gas-adjustment 1.4 \
--gas-prices 0.055uart \
--node http://localhost:31757 \
-y
```

#### Make sure you see the validator details and check

```bash
artelad q staking validator $(artelad keys show wallet --bech val -a)
```

### <mark style="color:purple;">Yes, You Are Now A Validator On The Arkeo Network</mark>

#### Don't Forget to save priv validator key

```bash
cat $HOME/.artelad/config/priv_validator_key.json
```
### Bizi takip edin [Twitter](https://twitter.com/corenodeHQ)
### Topluluğumuza katılın [Telegram](https://t.me/corenodechat)
