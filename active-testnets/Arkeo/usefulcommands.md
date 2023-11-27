# Useful Commands

##

## Key
 
### Add New Key

```
arkeod keys add wallet
```

### Recover Existing Key

```
arkeod keys add wallet --recover
```

### List All Keys

```
arkeod keys list
```
### Delete Keys

```
arkeod keys delete wallet
```

### Query Wallet Balance

```
arkeod q bank balances $(arkeod keys show wallet -a)
```

## Validator
 
### Create New Validator
```
arkeod tx staking create-validator \
--amount 1000000uarkeo \
--pubkey $(arkeod tendermint show-validator) \
--moniker "MONIKER_NAME" \
--identity "KEYBASE_ID" \
--details "DETAILS" \
--website "WEBSITE_URL" \
--chain-id arkeo \
--commission-rate 0.05 \
--commission-max-rate 0.20 \
--commission-max-change-rate 0.01 \
--min-self-delegation 1 \
--from wallet \
--gas-adjustment 1.5 \
--gas auto \
--gas-prices 0.1uarkeo \
-y
```

### Edit Existing Validator

```
arkeod tx staking edit-validator \
--new-moniker "MONIKER_NAME" \
--identity "KEYBASE_ID" \
--details "DETAILS" \
--website "WEBSITE_URL" \
--chain-id arkeo \
--commission-rate 0.05 \
--from wallet \
--gas-adjustment 1.5 \
--gas auto \
--gas-prices 0.1uarkeo \
-y
```

### Validator Details

```
arkeod q staking validator $(arkeod keys show wallet --bech val -a)
```

### Validator Unjail

```
arkeod tx slashing unjail --from wallet --chain-id arkeo --gas-adjustment 1.5 --gas auto --gas-prices 0.1uarkeo -y
```

### Jail Reason

```
arkeod query slashing signing-info $(arkeod tendermint show-validator)
```

### List All Active Validators

```
arkeod q staking validators -oj --limit=3000 | jq '.validators[] | select(.status==BOND_STATUS_BONDED)' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) +  t  + .description.moniker' | sort -gr | nl
```

### List All Inactive Validators

```
arkeod q staking validators -oj --limit=3000 | jq '.validators[] | select(.status==BOND_STATUS_UNBONDED)' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) +  t  + .description.moniker' | sort -gr | nl
```

## Token
 
### Send Token

```
arkeod tx bank send wallet <TO_WALLET_ADDRESS> 1000000uarkeo --from wallet --chain-id arkeo --gas-adjustment 1.5 --gas auto --gas-prices 0.1uarkeo -y
```

### Delegate

```
arkeod tx staking delegate <TO_VALOPER_ADDRESS> 1000000uarkeo --from wallet --chain-id arkeo --gas-adjustment 1.5 --gas auto --gas-prices 0.1uarkeo -y
```

### Delegate To Yourself

```
arkeod tx staking delegate $(arkeod keys show wallet --bech val -a) 1000000uarkeo --from wallet --chain-id arkeo --gas-adjustment 1.5 --gas auto --gas-prices 0.1uarkeo -y
```

### Redelegate

```
arkeod tx staking redelegate <FROM_VALOPER_ADDRESS> <TO_VALOPER_ADDRESS> 1000000uarkeo --from wallet --chain-id arkeo --gas-adjustment 1.5 --gas auto --gas-prices 0.1uarkeo -y
```

### Redelegate From Your Validator To Another

```
arkeod tx staking redelegate $(arkeod keys show wallet --bech val -a) <TO_VALOPER_ADDRESS> 1000000uarkeo --from wallet --chain-id arkeo --gas-adjustment 1.5 --gas auto --gas-prices 0.1uarkeo -y
```

### Unbond Tokens From Your Validator

```
arkeod tx staking unbond $(arkeod keys show wallet --bech val -a) 1000000uarkeo --from wallet --chain-id arkeo --gas-adjustment 1.5 --gas auto --gas-prices 0.1uarkeo -y
```

### Withdraw Rewards From All Validators

```
arkeod tx distribution withdraw-all-rewards --from wallet --chain-id arkeo --gas-adjustment 1.5 --gas auto --gas-prices 0.1uarkeo -y
```

### Withdraw Commission And Rewards From Your Validator

```
arkeod tx distribution withdraw-rewards $(arkeod keys show wallet --bech val -a) --commission --from wallet --chain-id arkeo --gas-adjustment 1.5 --gas auto --gas-prices 0.1uarkeo -y
```

## Governance 
 
### List All Proposals

```
arkeod query gov proposals
```

### View Proposal By ID

```
arkeod query gov proposal <ID>
```

### Vote Yes

```
arkeod tx gov vote <ID> yes --from wallet --chain-id arkeo --gas-adjustment 1.5 --gas auto --gas-prices 0.1uarkeo -y
```

### Vote No

```
arkeod tx gov vote <ID> no --from wallet --chain-id arkeo --gas-adjustment 1.5 --gas auto --gas-prices 0.1uarkeo -y
```

### Vote Abstain

```
arkeod tx gov vote <ID> abstain --from wallet --chain-id arkeo --gas-adjustment 1.5 --gas auto --gas-prices 0.1uarkeo -y
```

### Vote No With Veto

```
arkeod tx gov vote <ID> no_with_veto --from wallet --chain-id arkeo --gas-adjustment 1.5 --gas auto --gas-prices 0.1uarkeo -y
```

## Configuration Settings
 
### Pruning

```
sed -i \
  -e 's|^pruning *=.*|pruning = "custom"|' \
  -e 's|^pruning-keep-recent *=.*|pruning-keep-recent = "100"|' \
  -e 's|^pruning-keep-every *=.*|pruning-keep-every = "0"|' \
  -e 's|^pruning-interval *=.*|pruning-interval = "10"|' \
  $HOME/.arkeo/config/app.toml
```

### Enable Indexer

```
sed -i -e 's|^indexer *=.*|indexer = kv|' $HOME/.arkeo/config/config.toml
```

### Disable İndexer

```
sed -i -e 's|^indexer *=.*|indexer = null|' $HOME/.arkeo/config/config.toml
```

### Change Default Port

> ### CUSTOM_PORT=314

```
sed -i -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:${CUSTOM_PORT}58\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:${CUSTOM_PORT}57\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:${CUSTOM_PORT}60\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:${CUSTOM_PORT}56\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":${CUSTOM_PORT}66\"%" $HOME/.arkeo/config/config.toml
sed -i -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:${CUSTOM_PORT}17\"%; s%^address = \":8080\"%address = \":${CUSTOM_PORT}80\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:${CUSTOM_PORT}90\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:${CUSTOM_PORT}91\"%" $HOME/.arkeo/config/app.toml
```

## Set Minimum Gas Price

```
sed -i 's|^minimum-gas-prices *=.*|minimum-gas-prices = "0.0001uarkeo"|g' $HOME/.arkeo/config/app.toml
```

## Enable Prometheus

```
sed -i -e s/prometheus = false/prometheus = true/ $HOME/.arkeo/config/config.toml
```

## Reset Chain Data

```
arkeod tendermint unsafe-reset-all --keep-addr-book --home $HOME/.arkeo --keep-addr-book
```

## Status And Control
 
### Sync Status

```
arkeod status 2>&1 | jq .SyncInfo
```

### Validator Status

```
arkeod status 2>&1 | jq .ValidatorInfo
```

### Node Status

```
arkeod status 2>&1 | jq .NodeInfo
```

### Validator Key Control

```
[[ $(arkeod q staking validator $(arkeod keys show wallet --bech val -a) -oj | jq -r .consensus_pubkey.key) = $(arkeod status | jq -r .ValidatorInfo.PubKey.value) ]] && echo -e "\n\e[1m\e[32mTrue\e[0m\n" || echo -e "\n\e[1m\e[31mFalse\e[0m\n"
```

### Query TX

```
arkeod query tx <TX_ID>
```

### Get Node Peer

```
echo $(arkeod tendermint show-node-id)@$(curl -s ifconfig.me):$(cat $HOME/.arkeo/config/config.toml | sed -n '/Address to listen for incoming connection/{n;p;}' | sed 's/.*://; s/".*//')
```

### Get Live Peers

```
curl -sS http://localhost:31457/net_info | jq -r '.result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr)"' | awk -F ':' '{print $1":"$(NF)}'
```

## Service Management
 
Reload Service Configuration

```
sudo systemctl daemon-reload
```

Enable Service

```
sudo systemctl enable arkeod
```

Disable Service

```
sudo systemctl disable arkeod
```

Start Service

```
sudo systemctl start arkeod
```

Stop Service

```
sudo systemctl stop arkeod
```

Restart Service

```
sudo systemctl restart arkeod
```

Check Service Status

```
sudo systemctl status arkeod
```

Check Service Logs

```
sudo journalctl -u arkeod -f --no-hostname -o cat
```

## Remove Node
 
```
sudo systemctl stop arkeod && sudo systemctl disable arkeod && sudo rm /etc/systemd/system/arkeod.service && sudo systemctl daemon-reload && rm -rf $HOME/.arkeo && rm -rf $HOME/arkeo && sudo rm -rf $(which arkeod)
```
