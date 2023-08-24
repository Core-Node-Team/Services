# Useful Commands

##

## Key

### Add New Key

```
cascadiad keys add wallet
```

### Recover Existing Key

```
cascadiad keys add wallet --recover
```

### List All Keys

```
cascadiad keys list
```

### Delete Keys

```
cascadiad keys delete wallet
```

### Query Wallet Balance

```
cascadiad q bank balances $(cascadiad keys show wallet -a)
```

##

## Validator

### Create New Validator

```
cascadiad tx staking create-validator \
--amount 1000000aCC \
--pubkey $(cascadiad tendermint show-validator) \
--moniker "MONIKER_NAME" \
--identity "KEYBASE_ID" \
--details "DETAILS" \
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

### Edit Existing Validator

```
cascadiad tx staking edit-validator \
--new-moniker "MONIKER_NAME" \
--identity "KEYBASE_ID" \
--details "DETAILS" \
--website "WEBSITE_URL" \
--chain-id cascadia_6102-1 \
--commission-rate 0.05 \
--from wallet \
--gas-adjustment 1.5 \
--gas auto \
--gas-prices 0.1aCC \
-y
```

### Validator Details

```
cascadiad q staking validator $(cascadiad keys show wallet --bech val -a)
```

### Validator Unjail

```
cascadiad tx slashing unjail --from wallet --chain-id cascadia_6102-1 --gas-adjustment 1.5 --gas auto --gas-prices  -y
```

### Jail Reason

```
cascadiad query slashing signing-info $(cascadiad tendermint show-validator)
```

### List All Active Validators

```
cascadiad q staking validators -oj --limit=3000 | jq '.validators[] | select(.status==BOND_STATUS_BONDED)' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) +  t  + .description.moniker' | sort -gr | nl
```

### List All Inactive Validators

```
cascadiad q staking validators -oj --limit=3000 | jq '.validators[] | select(.status==BOND_STATUS_UNBONDED)' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) +  t  + .description.moniker' | sort -gr | nl
```

## Token

### Send Token

```
cascadiad tx bank send wallet <TO_WALLET_ADDRESS> 1000000aCC --from wallet --chain-id cascadia_6102-1 --gas-adjustment 1.5 --gas auto --gas-prices 0.1aCC -y
```

### Delegate

```
cascadiad tx staking delegate <TO_VALOPER_ADDRESS> 1000000aCC --from wallet --chain-id cascadia_6102-1 --gas-adjustment 1.5 --gas auto --gas-prices 0.1aCC -y
```

### Delegate To Yourself

```
cascadiad tx staking delegate $(cascadiad keys show wallet --bech val -a) 1000000aCC --from wallet --chain-id cascadia_6102-1 --gas-adjustment 1.5 --gas auto --gas-prices 0.1aCC -y
```

### Redelegate

```
cascadiad tx staking redelegate <FROM_VALOPER_ADDRESS> <TO_VALOPER_ADDRESS> 1000000aCC --from wallet --chain-id cascadia_6102-1 --gas-adjustment 1.5 --gas auto --gas-prices 0.1aCC -y
```

### Redelegate From Your Validator To Another

```
cascadiad tx staking redelegate $(cascadiad keys show wallet --bech val -a) <TO_VALOPER_ADDRESS> 1000000aCC --from wallet --chain-id cascadia_6102-1 --gas-adjustment 1.5 --gas auto --gas-prices 0.1aCC -y
```

### Unbond Tokens From Your Validator

```
cascadiad tx staking unbond $(cascadiad keys show wallet --bech val -a) 1000000aCC --from wallet --chain-id cascadia_6102-1 --gas-adjustment 1.5 --gas auto --gas-prices 0.1aCC -y
```

### Withdraw Rewards From All Validators

```
cascadiad tx distribution withdraw-all-rewards --from wallet --chain-id cascadia_6102-1 --gas-adjustment 1.5 --gas auto --gas-prices 0.1aCC -y
```

### Withdraw Commission And Rewards From Your Validator

```
cascadiad tx distribution withdraw-rewards $(cascadiad keys show wallet --bech val -a) --commission --from wallet --chain-id cascadia_6102-1 --gas-adjustment 1.5 --gas auto --gas-prices 0.1aCC -y
```

## Governance

### List All Proposals

```
cascadiad query gov proposals
```

### View Proposal By ID

```
cascadiad query gov proposal <ID>
```

### Vote Yes

```
cascadiad tx gov vote <ID> yes --from wallet --chain-id cascadia_6102-1 --gas-adjustment 1.5 --gas auto --gas-prices 0.1aCC -y
```

### Vote No

```
cascadiad tx gov vote <ID> no --from wallet --chain-id cascadia_6102-1 --gas-adjustment 1.5 --gas auto --gas-prices 0.1aCC -y
```

### Vote Abstain

```
cascadiad tx gov vote <ID> abstain --from wallet --chain-id cascadia_6102-1 --gas-adjustment 1.5 --gas auto --gas-prices 0.1aCC -y
```

### Vote No With Veto

```
cascadiad tx gov vote <ID> no_with_veto --from wallet --chain-id cascadia_6102-1 --gas-adjustment 1.5 --gas auto --gas-prices 0.1aCC -y
```

## Configuration Settings

### Pruning

```
sed -i \
  -e 's|^pruning *=.*|pruning = "custom"|' \
  -e 's|^pruning-keep-recent *=.*|pruning-keep-recent = "100"|' \
  -e 's|^pruning-keep-every *=.*|pruning-keep-every = "0"|' \
  -e 's|^pruning-interval *=.*|pruning-interval = "10"|' \
  $HOME/.cascadiad/config/app.toml
```

### Enable Indexer

```
sed -i -e 's|^indexer *=.*|indexer = kv|' $HOME/.cascadiad/config/config.toml
```

### Disable İndexer

```
sed -i -e 's|^indexer *=.*|indexer = null|' $HOME/.cascadiad/config/config.toml
```

### Change Default Port

> #### CUSTOM\_PORT=119

```
sed -i -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:${CUSTOM_PORT}58\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:${CUSTOM_PORT}57\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:${CUSTOM_PORT}60\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:${CUSTOM_PORT}56\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":${CUSTOM_PORT}66\"%" $HOME/.cascadiad/config/config.toml
sed -i -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:${CUSTOM_PORT}17\"%; s%^address = \":8080\"%address = \":${CUSTOM_PORT}80\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:${CUSTOM_PORT}90\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:${CUSTOM_PORT}91\"%" $HOME/.cascadiad/config/app.toml
```

### Set Minimum Gas Price

```
sed -i.bak -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.0025aCC\"/" ~/.cascadiad/config/app.toml
```

### Enable Prometheus

```
sed -i -e s/prometheus = false/prometheus = true/ $HOME/.cascadiad/config/config.toml
```

### Reset Chain Data

```
cascadiad tendermint unsafe-reset-all --keep-addr-book --home $HOME/.cascadiad --keep-addr-book
```

## Status And Control

### Sync Status

```
cascadiad status 2>&1 | jq .SyncInfo
```

### Validator Status

```
cascadiad status 2>&1 | jq .ValidatorInfo
```

### Node Sattus

```
cascadiad status 2>&1 | jq .NodeInfo
```

### Validator Key Control

```
[[ $(cascadiad q staking validator $(cascadiad keys show wallet --bech val -a) -oj | jq -r .consensus_pubkey.key) = $(cascadiad status | jq -r .ValidatorInfo.PubKey.value) ]] && echo -e "\n\e[1m\e[32mTrue\e[0m\n" || echo -e "\n\e[1m\e[31mFalse\e[0m\n"
```

### Query TX

```
cascadiad query tx <TX_ID>
```

### Get Node Peer

```
echo $(cascadiad tendermint show-node-id)@$(curl -s ifconfig.me):$(cat $HOME/.cascadiad/config/config.toml | sed -n '/Address to listen for incoming connection/{n;p;}' | sed 's/.*://; s/".*//')
```

### Get Live Peers

```
curl -sS http://localhost:11957/net_info | jq -r '.result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr)"' | awk -F ':' '{print $1":"$(NF)}'
```

## Service Management

Reload Service Configuration

```
sudo systemctl daemon-reload
```

Enable Service

```
sudo systemctl enable cascadiad
```

Disable Service

```
sudo systemctl disable cascadiad
```

Start Service

```
sudo systemctl start cascadiad
```

Stop Service

```
sudo systemctl stop cascadiad
```

Restart Service

```
sudo systemctl restart cascadiad
```

Check Service Status

```
sudo systemctl status cascadiad
```

Check Service Logs

```
sudo journalctl -u cascadiad -f --no-hostname -o cat
```

## Remove Node

```
sudo systemctl stop cascadiad && sudo systemctl disable cascadiad && sudo rm /etc/systemd/system/cascadiad.service && sudo systemctl daemon-reload && rm -rf $HOME/.cascadiad && rm -rf $HOME/cascadiad && sudo rm -rf $(which cascadiad)
```
