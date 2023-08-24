# Useful Commands

##

## Key

### Add New Key

```
ojod keys add wallet
```

### Recover Existing Key

```
ojod keys add wallet --recover
```

### List All Keys

```
ojod keys list
```

### Delete Keys

```
ojod keys delete wallet
```

### Query Wallet Balance

```
ojod q bank balances $(ojod keys show wallet -a)
```

##

## Validator

### Create New Validator

```
ojod tx staking create-validator \
--amount 1000000uojo \
--pubkey $(ojod tendermint show-validator) \
--moniker "MONIKER_NAME" \
--identity "KEYBASE_ID" \
--details "DETAILS" \
--website "WEBSITE_URL" \
--chain-id ojo-devnet \
--commission-rate 0.05 \
--commission-max-rate 0.20 \
--commission-max-change-rate 0.01 \
--min-self-delegation 1 \
--from wallet \
--gas-adjustment 1.5 \
--gas auto \
--gas-prices 0.1uojo \
-y
```

### Edit Existing Validator

```
ojod tx staking edit-validator \
--new-moniker "MONIKER_NAME" \
--identity "KEYBASE_ID" \
--details "DETAILS" \
--website "WEBSITE_URL" \
--chain-id ojo-devnet \
--commission-rate 0.05 \
--from wallet \
--gas-adjustment 1.5 \
--gas auto \
--gas-prices 0.1uojo \
-y
```

### Validator Details

```
ojod q staking validator $(ojod keys show wallet --bech val -a)
```

### Validator Unjail

```
ojod tx slashing unjail --from wallet --chain-id ojo-devnet --gas-adjustment 1.5 --gas auto --gas-prices 0.1uojo -y
```

### Jail Reason

```
ojod query slashing signing-info $(ojod tendermint show-validator)
```

### List All Active Validators

```
ojod q staking validators -oj --limit=3000 | jq '.validators[] | select(.status==BOND_STATUS_BONDED)' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) +  t  + .description.moniker' | sort -gr | nl
```

### List All Inactive Validators

```
ojod q staking validators -oj --limit=3000 | jq '.validators[] | select(.status==BOND_STATUS_UNBONDED)' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) +  t  + .description.moniker' | sort -gr | nl
```

## Token

### Send Token

```
ojod tx bank send wallet <TO_WALLET_ADDRESS> 1000000uojo --from wallet --chain-id ojo-devnet --gas-adjustment 1.5 --gas auto --gas-prices 0.1uojo -y
```

### Delegate

```
ojod tx staking delegate <TO_VALOPER_ADDRESS> 1000000uojo --from wallet --chain-id ojo-devnet --gas-adjustment 1.5 --gas auto --gas-prices 0.1uojo -y
```

### Delegate To Yourself

```
ojod tx staking delegate $(ojod keys show wallet --bech val -a) 1000000uojo --from wallet --chain-id ojo-devnet --gas-adjustment 1.5 --gas auto --gas-prices 0.1uojo -y
```

### Redelegate

```
ojod tx staking redelegate <FROM_VALOPER_ADDRESS> <TO_VALOPER_ADDRESS> 1000000uojo --from wallet --chain-id ojo-devnet --gas-adjustment 1.5 --gas auto --gas-prices 0.1uojo -y
```

### Redelegate From Your Validator To Another

```
ojod tx staking redelegate $(ojod keys show wallet --bech val -a) <TO_VALOPER_ADDRESS> 1000000uojo --from wallet --chain-id ojo-devnet --gas-adjustment 1.5 --gas auto --gas-prices 0.1uojo -y
```

### Unbond Tokens From Your Validator

```
ojod tx staking unbond $(ojod keys show wallet --bech val -a) 1000000uojo --from wallet --chain-id ojo-devnet --gas-adjustment 1.5 --gas auto --gas-prices 0.1uojo -y
```

### Withdraw Rewards From All Validators

```
ojod tx distribution withdraw-all-rewards --from wallet --chain-id ojo-devnet --gas-adjustment 1.5 --gas auto --gas-prices 0.1uojo -y
```

### Withdraw Commission And Rewards From Your Validator

```
ojod tx distribution withdraw-rewards $(ojod keys show wallet --bech val -a) --commission --from wallet --chain-id ojo-devnet --gas-adjustment 1.5 --gas auto --gas-prices 0.1uojo -y
```

## Governance

### List All Proposals

```
ojod query gov proposals
```

### View Proposal By ID

```
ojod query gov proposal <ID>
```

### Vote Yes

```
ojod tx gov vote <ID> yes --from wallet --chain-id ojo-devnet --gas-adjustment 1.5 --gas auto --gas-prices 0.1uojo -y
```

### Vote No

```
ojod tx gov vote <ID> no --from wallet --chain-id ojo-devnet --gas-adjustment 1.5 --gas auto --gas-prices 0.1uojo -y
```

### Vote Abstain

```
ojod tx gov vote <ID> abstain --from wallet --chain-id ojo-devnet --gas-adjustment 1.5 --gas auto --gas-prices 0.1uojo -y
```

### Vote No With Veto

```
ojod tx gov vote <ID> no_with_veto --from wallet --chain-id ojo-devnet --gas-adjustment 1.5 --gas auto --gas-prices 0.1uojo -y
```

## Configuration Settings

### Pruning

```
sed -i \
  -e 's|^pruning *=.*|pruning = "custom"|' \
  -e 's|^pruning-keep-recent *=.*|pruning-keep-recent = "100"|' \
  -e 's|^pruning-keep-every *=.*|pruning-keep-every = "0"|' \
  -e 's|^pruning-interval *=.*|pruning-interval = "10"|' \
  $HOME/.ojo/config/app.toml
```

### Enable Indexer

```
sed -i -e 's|^indexer *=.*|indexer = kv|' $HOME/.ojo/config/config.toml
```

### Disable İndexer

```
sed -i -e 's|^indexer *=.*|indexer = null|' $HOME/.ojo/config/config.toml
```

### Change Default Port

> #### CUSTOM\_PORT=312

```
sed -i -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:${CUSTOM_PORT}58\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:${CUSTOM_PORT}57\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:${CUSTOM_PORT}60\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:${CUSTOM_PORT}56\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":${CUSTOM_PORT}66\"%" $HOME/.ojo/config/config.toml
sed -i -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:${CUSTOM_PORT}17\"%; s%^address = \":8080\"%address = \":${CUSTOM_PORT}80\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:${CUSTOM_PORT}90\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:${CUSTOM_PORT}91\"%" $HOME/.ojo/config/app.toml
```

### Set Minimum Gas Price

```
sed -i 's|^minimum-gas-prices *=.*|minimum-gas-prices = "0.0001uojo"|g' $HOME/.ojo/config/app.toml
```

### Enable Prometheus

```
sed -i -e s/prometheus = false/prometheus = true/ $HOME/.ojo/config/config.toml
```

### Reset Chain Data

```
ojod tendermint unsafe-reset-all --keep-addr-book --home $HOME/.ojo --keep-addr-book
```

## Status And Control

### Sync Status

```
ojod status 2>&1 | jq .SyncInfo
```

### Validator Status

```
ojod status 2>&1 | jq .ValidatorInfo
```

### Node Status

```
ojod status 2>&1 | jq .NodeInfo
```

### Validator Key Control

```
[[ $(ojod q staking validator $(ojod keys show wallet --bech val -a) -oj | jq -r .consensus_pubkey.key) = $(ojod status | jq -r .ValidatorInfo.PubKey.value) ]] && echo -e "\n\e[1m\e[32mTrue\e[0m\n" || echo -e "\n\e[1m\e[31mFalse\e[0m\n"
```

### Query TX

```
ojod query tx <TX_ID>
```

### Get Node Peer

```
echo $(ojod tendermint show-node-id)@$(curl -s ifconfig.me):$(cat $HOME/.ojo/config/config.toml | sed -n '/Address to listen for incoming connection/{n;p;}' | sed 's/.*://; s/".*//')
```

### Get Live Peers

```
curl -sS http://localhost:31257/net_info | jq -r '.result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr)"' | awk -F ':' '{print $1":"$(NF)}'
```

## Service Management

Reload Service Configuration

```
sudo systemctl daemon-reload
```

Enable Service

```
sudo systemctl enable ojod
```

Disable Service

```
sudo systemctl disable ojod
```

Start Service

```
sudo systemctl start ojod
```

Stop Service

```
sudo systemctl stop ojod
```

Restart Service

```
sudo systemctl restart ojod
```

Check Service Status

```
sudo systemctl status ojod
```

Check Service Logs

```
sudo journalctl -u ojod -f --no-hostname -o cat
```

## Remove Node

```
sudo systemctl stop ojod && sudo systemctl disable ojod && sudo rm /etc/systemd/system/ojod.service && sudo systemctl daemon-reload && rm -rf $HOME/.ojo && rm -rf $HOME/ojo && sudo rm -rf $(which ojod)
```
