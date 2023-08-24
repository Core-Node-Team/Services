

<div align="center"

![cascadia](https://github.com/Core-Node-Team/Gitbook/assets/108215275/eeb809a7-d806-40ba-ad80-2f9e3d98ba0a)

 </div>

 
## For automatic installation, enter this command and follow the instructions
```
curl -sSl -o cascadia-kurulum.sh https://raw.githubusercontent.com/Core-Node-Team/Testnet-TR/main/Cascadia/cascadia.sh && chmod +x cascadia-kurulum.sh && bash ./cascadia-kurulum.sh
```
## For manuel installation follow this [guide](manuel-install.md)

## Become A Validator

### Create Keyring
* Don't forget to save the mnemonic
```
cascadiad keys add wallet
```
### Get Funds
Go to Cascadia [Discord](https://discord.gg/cascadia) and get test tokens from **#faucet** with `$faucet wallet address`

### Check Sync Status
* Should return ***False***
```
cascadiad status 2>&1 | jq .SyncInfo
```
## Create Validator
```
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

### Make sure you see the validator details and check
```
cascadiad q staking validator $(cascadiad keys show wallet --bech val -a)
```
## Yes, You Are Now A Validator On The Cascadia Network

### Don't Forget to save priv validator key
```
cat $HOME/.cascadiad/config/priv_validator_key.json
```






