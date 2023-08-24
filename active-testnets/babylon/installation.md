<h1 align="center"> Babylon | Testnet </h1>

<div align="center"
     

![babylon](https://github.com/Core-Node-Team/Gitbook/assets/108215275/fe72b6b0-bd7b-4c56-b94c-57c7c7e21eac)

 </div>

 
## For automatic installation, enter this command and follow the instructions
```
curl -sSL -o babylon-kurulum.sh https://raw.githubusercontent.com/Core-Node-Team/Testnet-TR/main/Babylon/babylon.sh && chmod +x babylon-kurulum.sh && bash ./babylon-kurulum.sh && source $HOME/.bash_profile
```
## For manuel installation follow this [guide](manuel-install.md)

## Become A Validator

### Create Keyring
* Don't forget to save the mnemonic
```
babylond keys add wallet
```
### Create BLS Key
```
babylond create-bls-key $(babylond keys show wallet -a)
```
* After creating a BLS key, you need to restart your node to load this key into memory.
```
sudo systemctl restart babylond
```

### Modify the Configuration
```
sed -i -e "s|^key-name *=.*|key-name = \"wallet\"|" $HOME/.babylond/config/app.toml

sed -i -e "s|^timeout_commit *=.*|timeout_commit = \"10s\"|" $HOME/.babylond/config/config.toml
```
### Get Funds
Go to Babylon [Discord](https://discord.gg/babylonglobal) and get test tokens from **#faucet** with `!faucet wallet address`


### Check Sync Status
* Should return ***False***
```
babylond status 2>&1 | jq .SyncInfo
```
## Create Validator
```
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
```
babylond q staking validator $(babylond keys show wallet --bech val -a)
```

## Yes, You Are Now A Validator On The Babylon Network

### Don't Forget to save priv validator key
```
cat $HOME/.babylond/config/priv_validator_key.json
```






