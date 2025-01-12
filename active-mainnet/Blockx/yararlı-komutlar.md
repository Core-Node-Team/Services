# Yararlı Komutlar

## Key

### Add New Key

```
blockxd keys add wallet
```

### Recover Existing Key

```
blockxd keys add wallet --recover
```

### List All Keys

```
blockxd keys list
```

### Delete Keys

```
blockxd keys delete wallet
```

### Query Wallet Balance

```
blockxd q bank balances $(blockxd keys show wallet -a)
```

##

## Validator

### Create New Validator

```
blockxd tx staking create-validator \
--amount 1000000abcx \
--pubkey $(blockxd tendermint show-validator) \
--moniker "MONIKER_NAME" \
--identity "KEYBASE_ID" \
--details "DETAILS" \
--website "WEBSITE_URL" \
--chain-id blockx_100-1 \
--commission-rate 0.05 \
--commission-max-rate 0.20 \
--commission-max-change-rate 0.01 \
--min-self-delegation 1 \
--from wallet \
--gas-adjustment 1.5 \
--gas auto \
--gas-prices 0abcx \
-y
```

### Edit Existing Validator

```
blockxd tx staking edit-validator \
--new-moniker "MONIKER_NAME" \
--identity "KEYBASE_ID" \
--details "DETAILS" \
--website "WEBSITE_URL" \
--chain-id blockx_100-1 \
--commission-rate 0.05 \
--from wallet \
--gas-adjustment 1.5 \
--gas auto \
--gas-prices 0abcx \
-y
```

### Validator Details

```
blockxd q staking validator $(blockxd keys show wallet --bech val -a)
```

### Validator Unjail

```
blockxd tx slashing unjail --from wallet --chain-id blockx_100-1 --gas-adjustment 1.5 --gas auto --gas-prices 0abcx -y
```

### Jail Reason

```
blockxd query slashing signing-info $(blockxd tendermint show-validator)
```

### List All Active Validators

```
blockxd q staking validators -oj --limit=3000 | jq '.validators[] | select(.status==BOND_STATUS_BONDED)' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) +  t  + .description.moniker' | sort -gr | nl
```

### List All Inactive Validators

```
blockxd q staking validators -oj --limit=3000 | jq '.validators[] | select(.status==BOND_STATUS_UNBONDED)' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) +  t  + .description.moniker' | sort -gr | nl
```

## Token

### Send Token

```
blockxd tx bank send wallet <TO_WALLET_ADDRESS> 1000000abcx --from wallet --chain-id blockx_100-1 --gas-adjustment 1.5 --gas auto --gas-prices 0abcx -y
```

### Delegate

```
blockxd tx staking delegate <TO_VALOPER_ADDRESS> 1000000abcx --from wallet --chain-id blockx_100-1 --gas-adjustment 1.5 --gas auto --gas-prices 0abcx -y
```

### Delegate To Yourself

```
blockxd tx staking delegate $(blockxd keys show wallet --bech val -a) 1000000abcx --from wallet --chain-id blockx_100-1 --gas-adjustment 1.5 --gas auto --gas-prices 0abcx -y
```

### Redelegate

```
blockxd tx staking redelegate <FROM_VALOPER_ADDRESS> <TO_VALOPER_ADDRESS> 1000000abcx --from wallet --chain-id blockx_100-1 --gas-adjustment 1.5 --gas auto --gas-prices 0abcx -y
```

### Redelegate From Your Validator To Another

```
blockxd tx staking redelegate $(blockxd keys show wallet --bech val -a) <TO_VALOPER_ADDRESS> 1000000abcx --from wallet --chain-id blockx_100-1 --gas-adjustment 1.5 --gas auto --gas-prices 0abcx -y
```

### Unbond Tokens From Your Validator

```
blockxd tx staking unbond $(blockxd keys show wallet --bech val -a) 1000000abcx --from wallet --chain-id blockx_100-1 --gas-adjustment 1.5 --gas auto --gas-prices 0abcx -y
```

### Withdraw Rewards From All Validators

```
blockxd tx distribution withdraw-all-rewards --from wallet --chain-id blockx_100-1 --gas-adjustment 1.5 --gas auto --gas-prices 0abcx -y
```

### Withdraw Commission And Rewards From Your Validator

```
blockxd tx distribution withdraw-rewards $(blockxd keys show wallet --bech val -a) --commission --from wallet --chain-id blockx_100-1 --gas-adjustment 1.5 --gas auto --gas-prices 0abcx -y
```

## Governance

### List All Proposals

```
blockxd query gov proposals
```

### View Proposal By ID

```
blockxd query gov proposal <ID>
```

### Vote Yes

```
blockxd tx gov vote <ID> yes --from wallet --chain-id blockx_100-1 --gas-adjustment 1.5 --gas auto --gas-prices 0abcx -y
```

### Vote No

```
blockxd tx gov vote <ID> no --from wallet --chain-id blockx_100-1 --gas-adjustment 1.5 --gas auto --gas-prices 0abcx -y
```

### Vote Abstain

```
blockxd tx gov vote <ID> abstain --from wallet --chain-id blockx_100-1 --gas-adjustment 1.5 --gas auto --gas-prices 0abcx -y
```

### Vote No With Veto

```
blockxd tx gov vote <ID> no_with_veto --from wallet --chain-id blockx_100-1 --gas-adjustment 1.5 --gas auto --gas-prices 0abcx -y
```

## Configuration Settings

### Pruning

```
sed -i \
  -e 's|^pruning *=.*|pruning = "custom"|' \
  -e 's|^pruning-keep-recent *=.*|pruning-keep-recent = "100"|' \
  -e 's|^pruning-keep-every *=.*|pruning-keep-every = "0"|' \
  -e 's|^pruning-interval *=.*|pruning-interval = "10"|' \
  $HOME/blockxd/config/app.toml
```

### Enable Indexer

```
sed -i -e 's|^indexer *=.*|indexer = kv|' $HOME/blockxd/config/config.toml
```

### Disable İndexer

```
sed -i -e 's|^indexer *=.*|indexer = null|' $HOME/blockxd/config/config.toml
```

### Port Değiştir

> #### CUSTOM\_PORT=122

```
sed -i -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:${CUSTOM_PORT}58\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:${CUSTOM_PORT}57\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:${CUSTOM_PORT}60\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:${CUSTOM_PORT}56\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":${CUSTOM_PORT}66\"%" $HOME/blockxd/config/config.toml
sed -i -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:${CUSTOM_PORT}17\"%; s%^address = \":8080\"%address = \":${CUSTOM_PORT}80\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:${CUSTOM_PORT}90\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:${CUSTOM_PORT}91\"%" $HOME/blockxd/config/app.toml
```

### Set Minimum Gas Price

```
```

### Enable Prometheus

```
sed -i -e s/prometheus = false/prometheus = true/ $HOME/blockxd/config/config.toml
```

### Reset Chain Data

```
blockxd tendermint unsafe-reset-all --keep-addr-book --home $HOME/blockxd --keep-addr-book
```

## Status And Control

### Sync Status

```
blockxd status 2>&1 | jq .SyncInfo
```

### Validator Status

```
blockxd status 2>&1 | jq .ValidatorInfo
```

### Node Sattus

```
blockxd status 2>&1 | jq .NodeInfo
```

### Validator Key Control

```
[[ $(blockxd q staking validator $(blockxd keys show wallet --bech val -a) -oj | jq -r .consensus_pubkey.key) = $(blockxd status | jq -r .ValidatorInfo.PubKey.value) ]] && echo -e "\n\e[1m\e[32mTrue\e[0m\n" || echo -e "\n\e[1m\e[31mFalse\e[0m\n"
```

### Query TX

```
blockxd query tx <TX_ID>
```

### Get Node Peer

```
echo $(blockxd tendermint show-node-id)@$(curl -s ifconfig.me):$(cat $HOME/blockxd/config/config.toml | sed -n '/Address to listen for incoming connection/{n;p;}' | sed 's/.*://; s/".*//')
```

### Get Live Peers

```
curl -sS http://localhost:12257/net_info | jq -r '.result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr)"' | awk -F ':' '{print $1":"$(NF)}'
```

## Service Management

Reload Service Configuration

```
sudo systemctl daemon-reload
```

Enable Service

```
sudo systemctl enable blockxd
```

Disable Service

```
sudo systemctl disable blockxd
```

Start Service

```
sudo systemctl start blockxd
```

Stop Service

```
sudo systemctl stop blockxd
```

Restart Service

```
sudo systemctl restart blockxd
```

Check Service Status

```
sudo systemctl status blockxd
```

Check Service Logs

```
sudo journalctl -u blockxd -f --no-hostname -o cat
```

## Remove Node

```
sudo systemctl stop blockxd && sudo systemctl disable blockxd && sudo rm /etc/systemd/system/blockxd.service && sudo systemctl daemon-reload && rm -rf $HOME/blockxd && rm -rf $HOME/blockx && sudo rm -rf $(which blockxd)
```
