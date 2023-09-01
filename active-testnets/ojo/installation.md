# Installation

![ojo](https://github.com/Core-Node-Team/Gitbook/assets/108215275/22d4b0aa-b9a6-4c87-9ed6-5278c0f7a13f)

<table data-full-width="false"><thead><tr><th align="center">Chain-ID</th><th align="center">Latest Version</th><th align="center">Custom Port</th></tr></thead><tbody><tr><td align="center"><mark style="color:orange;">ojo-devnet</mark></td><td align="center"><mark style="color:green;">HEAD-ad5a2377134aa13d7d76575b95613cf8ed12d1e4</mark></td><td align="center"><mark style="color:yellow;">312</mark></td></tr></tbody></table>

> ## Hardware Requirements
<table data-header-hidden data-full-width="false"><thead><tr><th width="247">Hardware Requirements</th><th> </th></tr></thead><tbody><tr><td>Minimum</td><td>4CPU 8RAM 100GB</td></tr><tr><td>Recommended</td><td>4CPU 16RAM 200GB</td></tr></tbody></table>

### For automatic installation, enter this command and follow the instructions

```bash
curl -sSL -o ojo.sh https://raw.githubusercontent.com/Core-Node-Team/Testnet-Guides/main/Ojo/ojo.sh && chmod +x ojo.sh && bash ./ojo.sh && source $HOME/.bash_profile && rm ojo.sh
```

### For manuel installation follow this [guide](manuel-install.md)

### Become A Validator

**Create Keyring**

* Don't forget to save the mnemonic

```bash
ojod keys add wallet
```

#### Get Funds

Go to Ojo [Discord](https://discord.gg/cascadia) and ask for test tokens to moderators

#### Check Sync Status

* Should return _<mark style="color:green;">**False**</mark>_

```bash
ojod status 2>&1 | jq .SyncInfo
```

#### Create Validator

```bash
ojod tx staking create-validator \
--amount 1000000uojo \
--pubkey $(ojod tendermint show-validator) \
--moniker "MONIKER_NAME" \
--identity "KEYBASE_ID" \
--details "Core Node Community" \
--website "WEBSITE_URL" \
--chain-id ojo-devnet \
--commission-rate 0.1 \
--commission-max-rate 0.20 \
--commission-max-change-rate 0.02 \
--min-self-delegation 1 \
--from wallet \
--gas-adjustment 1.5 \
--gas auto \
--gas-prices 0.1uojo \
-y
```

#### Make sure you see the validator details and check

```bash
ojod q staking validator $(ojod keys show wallet --bech val -a)
```

### <mark style="color:purple;">Yes, You Are Now A Validator On The Ojo Network</mark>

#### Don't Forget to save priv validator key

```bash
cat $HOME/.ojo/config/priv_validator_key.json
```
