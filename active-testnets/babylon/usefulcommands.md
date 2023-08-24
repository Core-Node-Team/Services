# Useful Commands

##

## Key

### Add New Key

{% code fullWidth="false" %}
```bash
babylond keys add wallet
```
{% endcode %}

### Recover Existing Key

```bash
babylond keys add wallet --recover
```

### List All Keys

```bash
babylond keys list
```

### Delete Keys

```bash
babylond keys delete wallet
```

### Query Wallet Balance

```bash
babylond q bank balances $(babylond keys show wallet -a)
```

##

## Validator

### Create New Validator

```go
babylond tx checkpointing create-validator \
--amount 1000000ubbn \
--pubkey $(babylond tendermint show-validator) \
--moniker "MONIKER_NAME" \
--identity "KEYBASE_ID" \
--details "DETAILS" \
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

### Edit Existing Validator

```go
babylond tx checkpointing edit-validator \
--new-moniker "MONIKER_NAME" \
--identity "KEYBASE_ID" \
--details "DETAILS" \
--website "WEBSITE_URL" \
--chain-id bbn-test-2 \
--commission-rate 0.05 \
--from wallet \
--gas-adjustment 1.5 \
--gas auto \
--gas-prices 0.1ubbn \
-y
```

### Validator Details

```bash
babylond q staking validator $(babylond keys show wallet --bech val -a)
```

### Validator Unjail

```go
babylond tx slashing unjail --from wallet --chain-id bbn-test-2 --gas-adjustment 1.5 --gas auto --gas-prices 0.1ubbn -y
```

### Jail Reason

```bash
babylond query slashing signing-info $(babylond tendermint show-validator)
```

### List All Active Validators

```bash
babylond q staking validators -oj --limit=3000 | jq '.validators[] | select(.status==BOND_STATUS_BONDED)' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) +  t  + .description.moniker' | sort -gr | nl
```

### List All Inactive Validators

```bash
babylond q staking validators -oj --limit=3000 | jq '.validators[] | select(.status==BOND_STATUS_UNBONDED)' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) +  t  + .description.moniker' | sort -gr | nl
```

## Token

### Send Token

```go
babylond tx bank send wallet <TO_WALLET_ADDRESS> 1000000ubbn --from wallet --chain-id bbn-test-2 --gas-adjustment 1.5 --gas auto --gas-prices 0.1ubbn -y
```

### Delegate

```go
babylond tx epoching delegate <TO_VALOPER_ADDRESS> 1000000ubbn --from wallet --chain-id bbn-test-2 --gas-adjustment 1.5 --gas auto --gas-prices 0.1ubbn -y
```

### Delegate To Yourself

```
babylond tx epoching delegate $(babylond keys show wallet --bech val -a) 1000000ubbn --from wallet --chain-id bbn-test-2 --gas-adjustment 1.5 --gas auto --gas-prices 0.1ubbn -y
```

### Redelegate

```
babylond tx epoching redelegate <FROM_VALOPER_ADDRESS> <TO_VALOPER_ADDRESS> 1000000ubbn --from wallet --chain-id bbn-test-2 --gas-adjustment 1.5 --gas auto --gas-prices 0.1ubbn -y
```

### Redelegate From Your Validator To Another

```
babylond tx epoching redelegate $(babylond keys show wallet --bech val -a) <TO_VALOPER_ADDRESS> 1000000ubbn --from wallet --chain-id bbn-test-2 --gas-adjustment 1.5 --gas auto --gas-prices 0.1ubbn -y
```

### Unbond Tokens From Your Validator

```
babylond tx epoching unbond $(babylond keys show wallet --bech val -a) 1000000ubbn --from wallet --chain-id bbn-test-2 --gas-adjustment 1.5 --gas auto --gas-prices 0.1ubbn -y
```

### Withdraw Rewards From All Validators

```
babylond tx distribution withdraw-all-rewards --from wallet --chain-id bbn-test-2 --gas-adjustment 1.5 --gas auto --gas-prices 0.1ubbn -y
```

### Withdraw Commission And Rewards From Your Validator

```
babylond tx distribution withdraw-rewards $(babylond keys show wallet --bech val -a) --commission --from wallet --chain-id bbn-test-2 --gas-adjustment 1.5 --gas auto --gas-prices 0.1ubbn -y
```

## Governance

### List All Proposals

```
babylond query gov proposals
```

### View Proposal By ID

```
babylond query gov proposal <ID>
```

### Vote Yes

```
babylond tx gov vote <ID> yes --from wallet --chain-id bbn-test-2 --gas-adjustment 1.5 --gas auto --gas-prices 0.1ubbn -y
```

### Vote No

```
babylond tx gov vote <ID> no --from wallet --chain-id bbn-test-2 --gas-adjustment 1.5 --gas auto --gas-prices 0.1ubbn -y
```

### Vote Abstain

```
babylond tx gov vote <ID> abstain --from wallet --chain-id bbn-test-2 --gas-adjustment 1.5 --gas auto --gas-prices 0.1ubbn -y
```

### Vote No With Veto

```
babylond tx gov vote <ID> no_with_veto --from wallet --chain-id bbn-test-2 --gas-adjustment 1.5 --gas auto --gas-prices 0.1ubbn -y
```

## Configuration Settings

### Pruning

```
sed -i \
  -e 's|^pruning *=.*|pruning = "custom"|' \
  -e 's|^pruning-keep-recent *=.*|pruning-keep-recent = "100"|' \
  -e 's|^pruning-keep-every *=.*|pruning-keep-every = "0"|' \
  -e 's|^pruning-interval *=.*|pruning-interval = "10"|' \
  $HOME/.babylond/config/app.toml
```

### Enable Indexer

```
sed -i -e 's|^indexer *=.*|indexer = kv|' $HOME/.babylond/config/config.toml
```

### Disable İndexer

```
sed -i -e 's|^indexer *=.*|indexer = null|' $HOME/.babylond/config/config.toml
```

### Change Default Port

> #### CUSTOM\_PORT=311

```
sed -i -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:${CUSTOM_PORT}58\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:${CUSTOM_PORT}57\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:${CUSTOM_PORT}60\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:${CUSTOM_PORT}56\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":${CUSTOM_PORT}66\"%" $HOME/.babylond/config/config.toml
sed -i -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:${CUSTOM_PORT}17\"%; s%^address = \":8080\"%address = \":${CUSTOM_PORT}80\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:${CUSTOM_PORT}90\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:${CUSTOM_PORT}91\"%" $HOME/.babylond/config/app.toml
```

### Set Minimum Gas Price

```
sed -i -e "s|^minimum-gas-prices *=.*|minimum-gas-prices = \"0.00001ubbn\"|" $HOME/.babylond/config/app.toml
```

### Enable Prometheus

```
sed -i -e s/prometheus = false/prometheus = true/ $HOME/.babylond/config/config.toml
```

### Reset Chain Data

```
babylond tendermint unsafe-reset-all --keep-addr-book --home $HOME/.babylond --keep-addr-book
```

## Status And Control

### Sync Status

```
babylond status 2>&1 | jq .SyncInfo
```

### Validator Status

```
babylond status 2>&1 | jq .ValidatorInfo
```

### Node Status

```
babylond status 2>&1 | jq .NodeInfo
```

### Validator Key Control

```
[[ $(babylond q staking validator $(babylond keys show wallet --bech val -a) -oj | jq -r .consensus_pubkey.key) = $(babylond status | jq -r .ValidatorInfo.PubKey.value) ]] && echo -e "\n\e[1m\e[32mTrue\e[0m\n" || echo -e "\n\e[1m\e[31mFalse\e[0m\n"
```

### Query TX

```
babylond query tx <TX_ID>
```

### Get Node Peer

```
echo $(babylond tendermint show-node-id)@$(curl -s ifconfig.me):$(cat $HOME/.babylond/config/config.toml | sed -n '/Address to listen for incoming connection/{n;p;}' | sed 's/.*://; s/".*//')
```

### Get Live Peers

```
curl -sS http://localhost:31157/net_info | jq -r '.result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr)"' | awk -F ':' '{print $1":"$(NF)}'
```

## Service Management

Reload Service Configuration

```
sudo systemctl daemon-reload
```

Enable Service

```
sudo systemctl enable babylond
```

Disable Service

```
sudo systemctl disable babylond
```

Start Service

```
sudo systemctl start babylond
```

Stop Service

```
sudo systemctl stop babylond
```

Restart Service

```
sudo systemctl restart babylond
```

Check Service Status

```
sudo systemctl status babylond
```

Check Service Logs

```
sudo journalctl -u babylond -f --no-hostname -o cat
```

## Remove Node


```
sudo systemctl stop babylond && sudo systemctl disable babylond && sudo rm /etc/systemd/system/babylond.service && sudo systemctl daemon-reload && rm -rf $HOME/.babylond && rm -rf $HOME/babylon && sudo rm -rf $(which babylond)
```
