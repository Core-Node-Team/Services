# Yararlı Komutlar

## Cüzdan Yönetimi

### Cüzdan Oluştur

```
crossfid keys add wallet
```

### Cüzdan Recover Et

```
crossfid keys add wallet --recover
```

### Cüzdanları Listele

```
crossfid keys list
```

### Cüzdan Sil

```
crossfid keys delete wallet
```

### Cüzdan Bakiyesini Sorgula

```
crossfid q bank balances $(crossfid keys show wallet -a)
```

##

## Validatör Yönetimi

### Validatör Oluştur

```
crossfid tx staking create-validator \
--amount 1000000mpx \
--pubkey $(crossfid tendermint show-validator) \
--moniker "MONİKER_İSMİNİZ" \
--identity "KEYBASE.İO_İD" \
--details "DETAYLAR" \
--website "WEBSİTE_LİNKİNİZ" \
--chain-id crossfi-evm-testnet-1 \
--commission-rate 0.05 \
--commission-max-rate 0.20 \
--commission-max-change-rate 0.01 \
--min-self-delegation 1 \
--from wallet \
--gas-adjustment 1.5 \
--gas auto \
--gas-prices 5000000000mpx \
-y
```

### Validatörü Düzenle

```
crossfid tx staking edit-validator \
--new-moniker "MONİKER_İSMİNİZ" \
--identity "KEYBASE.İO_İD" \
--details "DETAYLAR" \
--website "WEBSİTE_LİNKİNİZ" \
--chain-id crossfi-evm-testnet-1 \
--commission-rate 0.05 \
--from wallet \
--gas-adjustment 1.5 \
--gas auto \
--gas-prices 5000000000mpx \
-y
```

### Validatör Detayları

```
crossfid q staking validator $(crossfid keys show wallet --bech val -a)
```

### Validatör Unjail

```
crossfid tx slashing unjail --from wallet --chain-id crossfi-evm-testnet-1 --gas-adjustment 1.5 --gas auto --gas-prices 5000000000mpx -y
```

### Jail Olma Sebebi

```
crossfid query slashing signing-info $(crossfid tendermint show-validator)
```

### Tüm Aktif Validatörleri Listele

```
crossfid q staking validators -oj --limit=3000 | jq '.validators[] | select(.status==BOND_STATUS_BONDED)' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) +  t  + .description.moniker' | sort -gr | nl
```

### Tüm İnaktif Validatörleri Listele

```
crossfid q staking validators -oj --limit=3000 | jq '.validators[] | select(.status==BOND_STATUS_UNBONDED)' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) +  t  + .description.moniker' | sort -gr | nl
```

## Token

### Token Gönder

```
crossfid tx bank send wallet <HEDEF_CÜZDAN_ADRESİ> 1000000mpx --from wallet --chain-id crossfi-evm-testnet-1 --gas-adjustment 1.5 --gas auto --gas-prices 5000000000mpx -y
```

### Delegate

```
crossfid tx staking delegate <VALOPER_ADRESİ> 1000000mpx --from wallet --chain-id crossfi-evm-testnet-1 --gas-adjustment 1.5 --gas auto --gas-prices 5000000000mpx -y
```

### Kendi Validatörüne Delegate

```
crossfid tx staking delegate $(crossfid keys show wallet --bech val -a) 1000000mpx --from wallet --chain-id crossfi-evm-testnet-1 --gas-adjustment 1.5 --gas auto ---gas-prices 5000000000mpx -y
```

### Redelegate

```
crossfid tx staking redelegate <İLK_VALOPER_ADRESİ> <HEDEF_VALOPER_ADRESİ> 1000000mpx --from wallet --chain-id crossfi-evm-testnet-1 --gas-adjustment 1.5 --gas auto --gas-prices 5000000000mpx -y
```

### Kendi Validatöründen Başka Validatöre Redelegate

```
crossfid tx staking redelegate $(crossfid keys show wallet --bech val -a) <VALOPER_ADRESİ> 1000000mpx --from wallet --chain-id crossfi-evm-testnet-1 --gas-adjustment 1.5 --gas auto --gas-prices 5000000000mpx -y
```

### Unbond

```
crossfid tx staking unbond $(crossfid keys show wallet --bech val -a) 1000000mpx --from wallet --chain-id crossfi-evm-testnet-1 --gas-adjustment 1.5 --gas auto --gas-prices 5000000000mpx -y
```

### Tüm Validatörlerden Komisyon ve Ödülleri Çekme

```
crossfid tx distribution withdraw-all-rewards --commission --from wallet --chain-id crossfi-evm-testnet-1 --gas-adjustment 1.5 --gas auto --gas-prices 5000000000mpx -y
```

### Kendi Validatörünüze Ait Komisyon ve Ödülleri Çekme

```
crossfid tx distribution withdraw-rewards $(crossfid keys show wallet --bech val -a) --commission --from wallet --chain-id crossfi-evm-testnet-1 --gas-adjustment 1.5 --gas auto --gas-prices 5000000000mpx -y
```

## Yönetim

### Tüm Oylamaları Görüntüle

```
crossfid query gov proposals
```

### Oylama Detaylarını Görüntüle

```
crossfid query gov proposal <ID>
```

### Evet Oyu Ver

```
crossfid tx gov vote <ID> yes --from wallet --chain-id crossfi-evm-testnet-1 --gas-adjustment 1.5 --gas auto --gas-prices 5000000000mpx -y
```

### Hayır Oyu Ver

```
crossfid tx gov vote <ID> no --from wallet --chain-id crossfi-evm-testnet-1 --gas-adjustment 1.5 --gas auto --gas-prices 5000000000mpx -y
```

### Çekimser Oyu Ver

```
crossfid tx gov vote <ID> abstain --from wallet --chain-id crossfi-evm-testnet-1 --gas-adjustment 1.5 --gas auto --gas-prices 5000000000mpx -y
```

### Hayır Oyu ve Veto Et

```
crossfid tx gov vote <ID> no_with_veto --from wallet --chain-id crossfi-evm-testnet-1 --gas-adjustment 1.5 --gas auto --gas-prices 5000000000mpx -y
```

## Yapılandırma Ayarları

### Pruning

```
sed -i \
  -e 's|^pruning *=.*|pruning = "custom"|' \
  -e 's|^pruning-keep-recent *=.*|pruning-keep-recent = "100"|' \
  -e 's|^pruning-keep-every *=.*|pruning-keep-every = "0"|' \
  -e 's|^pruning-interval *=.*|pruning-interval = "10"|' \
  $HOME/.mineplex-chain/config/app.toml
```

### İndexer Aç

```
sed -i -e 's|^indexer *=.*|indexer = kv|' $HOME/.mineplex-chain/config/config.toml
```

### İndexer Kapat

```
sed -i -e 's|^indexer *=.*|indexer = null|' $HOME/.mineplex-chain/config/config.toml
```

### Port Değiştir

> #### Port=366

```
sed -i -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:36658\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:36657\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:36660\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:36656\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":36666\"%" $HOME/.mineplex-chain/config/config.toml
sed -i -e "s%^address = \"tcp://localhost:1317\"%address = \"tcp://localhost:36617\"%; s%^address = \":8080\"%address = \":36680\"%; s%^address = \"localhost:9090\"%address = \"localhost:36690\"%; s%^address = \"localhost:9091\"%address = \"localhost:36691\"%; s%:8545%:36645%; s%:8546%:36646%; s%:6065%:36665%" $HOME/.mineplex-chain/config/app.toml
```

### Min Gas Price Ayarla

```
```

### Prometheus Aktif Et

```
sed -i -e s/prometheus = false/prometheus = true/ $HOME/.mineplex-chain/config/config.toml
```

### Zincir Verilerini Sıfırla

```
crossfid tendermint unsafe-reset-all --keep-addr-book --home $HOME/.mineplex-chain --keep-addr-book
```

## Durum Sorgulama ve Kontrol

### Senkronizasyon Durumu

```
crossfid status 2>&1 | jq .SyncInfo
```

### Validatör Durumu

```
crossfid status 2>&1 | jq .ValidatorInfo
```

### Node Durumu

```
crossfid status 2>&1 | jq .NodeInfo
```

### Validatör Key Kontrol

```
[[ $(crossfid q staking validator $(crossfid keys show wallet --bech val -a) -oj | jq -r .consensus_pubkey.key) = $(crossfid status | jq -r .ValidatorInfo.PubKey.value) ]] && echo -e "\n\e[1m\e[32mTrue\e[0m\n" || echo -e "\n\e[1m\e[31mFalse\e[0m\n"
```

### TX Sorgulama

```
crossfid query tx <TX_ID>
```

### Peer Adresini Öğren

```
echo $(crossfid tendermint show-node-id)@$(curl -s ifconfig.me):$(cat $HOME/.mineplex-chain/config/config.toml | sed -n '/Address to listen for incoming connection/{n;p;}' | sed 's/.*://; s/".*//')
```

### Bağlı Peerleri Öğren

```
curl -sS http://localhost:36657/net_info | jq -r ".result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr)"" | awk -F ":" "{print $1":"$NF}"
```

## Service Yönetimi

Servisi Etkinleştir

```
sudo systemctl enable crossfid
```

Servisi Devre Dışı Bırak

```
sudo systemctl disable crossfid
```

Servisi Başlat

```
sudo systemctl start crossfid
```

Servisi Durdur

```
sudo systemctl stop crossfid
```

Servisi Yeniden Başlat

```
sudo systemctl restart crossfid
```

Servis Durumunu Kontrol Et

```
sudo systemctl status crossfid
```

Servis Loglarını Kontrol Et

```
sudo journalctl -u crossfid -f --no-hostname -o cat
```

## Node Silmek

```
sudo systemctl stop crossfid && sudo systemctl disable crossfid && sudo rm /etc/systemd/system/crossfid.service && sudo systemctl daemon-reload && rm -rf $HOME/.mineplex-chain && rm -rf $HOME/crossfi && sudo rm -rf $(which crossfid)
```
