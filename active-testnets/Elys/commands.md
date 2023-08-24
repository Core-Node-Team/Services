 
#
<h1 align=center> Key </h1>
 
## Add New Key
```
elysd keys add wallet
```
## Recover Existing Key
```
elysd keys add wallet --recover
```
## List All Keys
```
elysd keys list
```
## Delete Keys
```
elysd keys delete wallet
```
## Query Wallet Balance
```
elysd q bank balances $(elysd keys show wallet -a)
```
#
<h1 align=center> Validator </h1>
 
## Create New Validator
```
elysd tx staking create-validator \
--amount 1000000uelys \
--pubkey $(elysd tendermint show-validator) \
--moniker "MONIKER_NAME" \
--identity "KEYBASE_ID" \
--details "DETAILS" \
--website "WEBSITE_URL" \
--chain-id elystestnet-1 \
--commission-rate 0.05 \
--commission-max-rate 0.20 \
--commission-max-change-rate 0.01 \
--min-self-delegation 1 \
--from wallet \
--gas-adjustment 1.5 \
--gas auto \
--gas-prices 1uelys \
-y
```
## Edit Existing Validator
```
elysd tx staking edit-validator \
--new-moniker "MONIKER_NAME" \
--identity "KEYBASE_ID" \
--details "DETAILS" \
--website "WEBSITE_URL" \
--chain-id elystestnet-1 \
--commission-rate 0.05 \
--from wallet \
--gas-adjustment 1.5 \
--gas auto \
--gas-prices 1uelys \
-y
```
## Validator Details
```
elysd q staking validator $(elysd keys show wallet --bech val -a)
```
## Validator Unjail
```
elysd tx slashing unjail --from wallet --chain-id elystestnet-1 --gas-adjustment 1.5 --gas auto --gas-prices 1uelys -y
```
## Jail Reason
```
elysd query slashing signing-info $(elysd tendermint show-validator)
```
## List All Active Validators
```
elysd q staking validators -oj --limit=3000 | jq '.validators[] | select(.status==BOND_STATUS_BONDED)' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) +  t  + .description.moniker' | sort -gr | nl
```
## List All Inactive Validators
```
elysd q staking validators -oj --limit=3000 | jq '.validators[] | select(.status==BOND_STATUS_UNBONDED)' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) +  t  + .description.moniker' | sort -gr | nl
```
<h1 align=center> Token </h1>
 
## Send Token
```
elysd tx bank send wallet <TO_WALLET_ADDRESS> 1000000uelys --from wallet --chain-id elystestnet-1 --gas-adjustment 1.5 --gas auto --gas-prices 1uelys -y
```
## Delegate
```
elysd tx staking delegate <TO_VALOPER_ADDRESS> 1000000uelys --from wallet --chain-id elystestnet-1 --gas-adjustment 1.5 --gas auto --gas-prices 1uelys -y
```
## Delegate To Yourself
```
elysd tx staking delegate $(elysd keys show wallet --bech val -a) 1000000uelys --from wallet --chain-id elystestnet-1 --gas-adjustment 1.5 --gas auto --gas-prices 1uelys -y
```
## Redelegate
```
elysd tx staking redelegate <FROM_VALOPER_ADDRESS> <TO_VALOPER_ADDRESS> 1000000uelys --from wallet --chain-id elystestnet-1 --gas-adjustment 1.5 --gas auto --gas-prices 1uelys -y
```
## Redelegate From Your Validator To Another
```
elysd tx staking redelegate $(elysd keys show wallet --bech val -a) <TO_VALOPER_ADDRESS> 1000000uelys --from wallet --chain-id elystestnet-1 --gas-adjustment 1.5 --gas auto --gas-prices 1uelys -y
```
## Unbond Tokens From Your Validator
```
elysd tx staking unbond $(elysd keys show wallet --bech val -a) 1000000uelys --from wallet --chain-id elystestnet-1 --gas-adjustment 1.5 --gas auto --gas-prices 1uelys -y
```
## Withdraw Rewards From All Validators
```
elysd tx distribution withdraw-all-rewards --from wallet --chain-id elystestnet-1 --gas-adjustment 1.5 --gas auto --gas-prices 1uelys -y
```
## Withdraw Commission And Rewards From Your Validator
```
elysd tx distribution withdraw-rewards $(elysd keys show wallet --bech val -a) --commission --from wallet --chain-id elystestnet-1 --gas-adjustment 1.5 --gas auto --gas-prices 1uelys -y
```
<h1 align=center> Governance </h1>
 
## List All Proposals
```
elysd query gov proposals
```
## View Proposal By ID
```
elysd query gov proposal <ID>
```
## Vote Yes
```
elysd tx gov vote <ID> yes --from wallet --chain-id elystestnet-1 --gas-adjustment 1.5 --gas auto --gas-prices 1uelys -y
```
## Vote No
```
elysd tx gov vote <ID> no --from wallet --chain-id elystestnet-1 --gas-adjustment 1.5 --gas auto --gas-prices 1uelys -y
```
## Vote Abstain
```
elysd tx gov vote <ID> abstain --from wallet --chain-id elystestnet-1 --gas-adjustment 1.5 --gas auto --gas-prices 1uelys -y
```
## Vote No With Veto
```
elysd tx gov vote <ID> no_with_veto --from wallet --chain-id elystestnet-1 --gas-adjustment 1.5 --gas auto --gas-prices 1uelys -y
```
<h1 align=center> Configuration Settings </h1>
 
 ## Pruning
```
sed -i \
  -e 's|^pruning *=.*|pruning = "custom"|' \
  -e 's|^pruning-keep-recent *=.*|pruning-keep-recent = "100"|' \
  -e 's|^pruning-keep-every *=.*|pruning-keep-every = "0"|' \
  -e 's|^pruning-interval *=.*|pruning-interval = "10"|' \
  $HOME/.elys/config/app.toml
```
## Enable Indexer
```
sed -i -e 's|^indexer *=.*|indexer = kv|' $HOME/.elys/config/config.toml
```
## Disable İndexer
```
sed -i -e 's|^indexer *=.*|indexer = null|' $HOME/.elys/config/config.toml
```
## Change Default Port
> ### CUSTOM_PORT=313
```
sed -i -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:${CUSTOM_PORT}58\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:${CUSTOM_PORT}57\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:${CUSTOM_PORT}60\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:${CUSTOM_PORT}56\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":${CUSTOM_PORT}66\"%" $HOME/.elys/config/config.toml
sed -i -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:${CUSTOM_PORT}17\"%; s%^address = \":8080\"%address = \":${CUSTOM_PORT}80\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:${CUSTOM_PORT}90\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:${CUSTOM_PORT}91\"%" $HOME/.elys/config/app.toml
```
## Set Minimum Gas Price
```
sed -i 's/minimum-gas-prices =.*/minimum-gas-prices = "0.0uelys"/g' $HOME/.elys/config/app.toml
```
## Enable Prometheus
```
sed -i -e s/prometheus = false/prometheus = true/ $HOME/.elys/config/config.toml
```
## Reset Chain Data
```
elysd tendermint unsafe-reset-all --keep-addr-book --home $HOME/.elys --keep-addr-book
```
<h1 align=center> Status And Control </h1>
 
## Sync Status
```
elysd status 2>&1 | jq .SyncInfo
```
## Validator Status
```
elysd status 2>&1 | jq .ValidatorInfo
```
## Node Status
```
elysd status 2>&1 | jq .NodeInfo
```
## Validator Key Control
```
[[ $(elysd q staking validator $(elysd keys show wallet --bech val -a) -oj | jq -r .consensus_pubkey.key) = $(elysd status | jq -r .ValidatorInfo.PubKey.value) ]] && echo -e "\n\e[1m\e[32mTrue\e[0m\n" || echo -e "\n\e[1m\e[31mFalse\e[0m\n"
```
## Query TX
```
elysd query tx <TX_ID>
```
## Get Node Peer
```
echo $(elysd tendermint show-node-id)@$(curl -s ifconfig.me):$(cat $HOME/.elys/config/config.toml | sed -n '/Address to listen for incoming connection/{n;p;}' | sed 's/.*://; s/".*//')
```
## Get Live Peers
```
curl -sS http://localhost:31357/net_info | jq -r '.result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr)"' | awk -F ':' '{print $1":"$(NF)}'
```
<h1 align=center> Service Management </h1>
 
Reload Service Configuration
```
sudo systemctl daemon-reload
```
Enable Service
```
sudo systemctl enable elysd
```
Disable Service
```
sudo systemctl disable elysd
```
Start Service
```
sudo systemctl start elysd
```
Stop Service
```
sudo systemctl stop elysd
```
Restart Service
```
sudo systemctl restart elysd
```
Check Service Status
```
sudo systemctl status elysd
```
Check Service Logs
```
sudo journalctl -u elysd -f --no-hostname -o cat
```
<h1 align=center> Remove Node </h1>
 
```
sudo systemctl stop elysd && sudo systemctl disable elysd && sudo rm /etc/systemd/system/elysd.service && sudo systemctl daemon-reload && rm -rf $HOME/.elys && rm -rf $HOME/elys && sudo rm -rf $(which elysd)
```

