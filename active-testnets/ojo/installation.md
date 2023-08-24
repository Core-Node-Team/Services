<div align="center">

![ojo](https://github.com/Core-Node-Team/Gitbook/assets/108215275/22d4b0aa-b9a6-4c87-9ed6-5278c0f7a13f)


</div>


## For automatic installation, enter this command and follow the instructions
```bash
curl -sSL -o ojo.sh https://raw.githubusercontent.com/0xSocrates/Testnet-Rehberler/main/Ojo/ojo.sh && chmod +x ojo.sh && bash ./ojo.sh
```
## For manuel installation follow this [guide](manuel-install.md)

## Become A Validator

#### Create Keyring

* Don't forget to save the mnemonic

```bash
ojod keys add wallet
```

### Get Funds

Go to Ojo [Discord](https://discord.gg/cascadia) and ask for test tokens to moderators

### Check Sync Status

* Should return _<mark style="color:green;">**False**</mark>_

```bash
ojod status 2>&1 | jq .SyncInfo
```
### Create Validator

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

### Make sure you see the validator details and check

```bash
ojod q staking validator $(ojod keys show wallet --bech val -a)
```

## Yes, You Are Now A Validator On The Ojo Network

### Don't Forget to save priv validator key

```bash
cat $HOME/.ojo/config/priv_validator_key.json
```


