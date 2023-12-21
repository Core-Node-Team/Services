 
#
<h1 align=center> Cüzdan Yönetimi </h1>
 
## Cüzdan Oluştur
```
pryzmd keys add wallet
```
## Cüzdan Recover Et
```
pryzmd keys add wallet --recover
```
## Cüzdanları Listele
```
pryzmd keys list
```
## Cüzdan Sil
```
pryzmd keys delete wallet
```
## Cüzdan Bakiyesini Sorgula
```
pryzmd q bank balances $(pryzmd keys show wallet -a)
```
#
<h1 align=center> Validatör Yönetimi </h1>
 
## Validatör Oluştur
```
pryzmd tx staking create-validator \
--amount 1000000upryzm \
--pubkey $(pryzmd tendermint show-validator) \
--moniker "MONİKER_İSMİNİZ" \
--identity "KEYBASE.İO_İD" \
--details "DETAYLAR" \
--website "WEBSİTE_LİNKİNİZ" \
--chain-id indigo-1 \
--commission-rate 0.05 \
--commission-max-rate 0.20 \
--commission-max-change-rate 0.01 \
--min-self-delegation 1 \
--from wallet \
--gas-adjustment 1.5 \
--gas auto \
--gas-prices 0.015upryzm \
-y
```
## Validatörü Düzenle
```
pryzmd tx staking edit-validator \
--new-moniker "MONİKER_İSMİNİZ" \
--identity "KEYBASE.İO_İD" \
--details "DETAYLAR" \
--website "WEBSİTE_LİNKİNİZ" \
--chain-id indigo-1 \
--commission-rate 0.05 \
--from wallet \
--gas-adjustment 1.5 \
--gas auto \
--gas-prices 0.015upryzm \
-y
```
## Validatör Detayları
```
pryzmd q staking validator $(pryzmd keys show wallet --bech val -a)
```
## Validatör Unjail
```
pryzmd tx slashing unjail --from wallet --chain-id indigo-1 --gas-adjustment 1.5 --gas auto --gas-prices 0.015upryzm -y
```
## Jail Olma Sebebi
```
pryzmd query slashing signing-info $(pryzmd tendermint show-validator)
```
## Tüm Aktif Validatörleri Listele
```
pryzmd q staking validators -oj --limit=3000 | jq '.validators[] | select(.status==BOND_STATUS_BONDED)' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) +  t  + .description.moniker' | sort -gr | nl
```
## Tüm İnaktif Validatörleri Listele
```
pryzmd q staking validators -oj --limit=3000 | jq '.validators[] | select(.status==BOND_STATUS_UNBONDED)' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) +  t  + .description.moniker' | sort -gr | nl
```
<h1 align=center> Token </h1>
 
## Token Gönder
```
pryzmd tx bank send wallet <HEDEF_CÜZDAN_ADRESİ> 1000000upryzm --from wallet --chain-id indigo-1 --gas-adjustment 1.5 --gas auto --gas-prices 0.015upryzm -y
```
## Delegate
```
pryzmd tx staking delegate <VALOPER_ADRESİ> 1000000upryzm --from wallet --chain-id indigo-1 --gas-adjustment 1.5 --gas auto --gas-prices 0.015upryzm -y
```
## Kendi Validatörüne Delegate
```
pryzmd tx staking delegate $(pryzmd keys show wallet --bech val -a) 1000000upryzm --from wallet --chain-id indigo-1 --gas-adjustment 1.5 --gas auto ---gas-prices 0.015upryzm -y
```
## Redelegate
```
pryzmd tx staking redelegate <İLK_VALOPER_ADRESİ> <HEDEF_VALOPER_ADRESİ> 1000000upryzm --from wallet --chain-id indigo-1 --gas-adjustment 1.5 --gas auto --gas-prices 0.015upryzm -y
```
## Kendi Validatöründen Başka Validatöre Redelegate
```
pryzmd tx staking redelegate $(pryzmd keys show wallet --bech val -a) <VALOPER_ADRESİ> 1000000upryzm --from wallet --chain-id indigo-1 --gas-adjustment 1.5 --gas auto --gas-prices 0.015upryzm -y
```
## Unbond
```
pryzmd tx staking unbond $(pryzmd keys show wallet --bech val -a) 1000000upryzm --from wallet --chain-id indigo-1 --gas-adjustment 1.5 --gas auto --gas-prices 0.015upryzm -y
```
## Tüm Validatörlerden Komisyon ve Ödülleri Çekme
```
pryzmd tx distribution withdraw-all-rewards --commission --from wallet --chain-id indigo-1 --gas-adjustment 1.5 --gas auto --gas-prices 0.015upryzm -y
```
## Kendi Validatörünüze Ait Komisyon ve Ödülleri Çekme
```
pryzmd tx distribution withdraw-rewards $(pryzmd keys show wallet --bech val -a) --commission --from wallet --chain-id indigo-1 --gas-adjustment 1.5 --gas auto --gas-prices 0.015upryzm -y
```
<h1 align=center> Yönetim </h1>
 
## Tüm Oylamaları Görüntüle
```
pryzmd query gov proposals
```
## Oylama Detaylarını Görüntüle
```
pryzmd query gov proposal <ID>
```
## Evet Oyu Ver
```
pryzmd tx gov vote <ID> yes --from wallet --chain-id indigo-1 --gas-adjustment 1.5 --gas auto --gas-prices 0.015upryzm -y
```
## Hayır Oyu Ver
```
pryzmd tx gov vote <ID> no --from wallet --chain-id indigo-1 --gas-adjustment 1.5 --gas auto --gas-prices 0.015upryzm -y
```
## Çekimser Oyu Ver
```
pryzmd tx gov vote <ID> abstain --from wallet --chain-id indigo-1 --gas-adjustment 1.5 --gas auto --gas-prices 0.015upryzm -y
```
## Hayır Oyu ve Veto Et
```
pryzmd tx gov vote <ID> no_with_veto --from wallet --chain-id indigo-1 --gas-adjustment 1.5 --gas auto --gas-prices 0.015upryzm -y
```
<h1 align=center> Yapılandırma Ayarları </h1>
 
 ## Pruning
```
sed -i \
  -e 's|^pruning *=.*|pruning = "custom"|' \
  -e 's|^pruning-keep-recent *=.*|pruning-keep-recent = "100"|' \
  -e 's|^pruning-keep-every *=.*|pruning-keep-every = "0"|' \
  -e 's|^pruning-interval *=.*|pruning-interval = "10"|' \
  $HOME/.pryzm/config/app.toml
```
## İndexer Aç
```
sed -i -e 's|^indexer *=.*|indexer = kv|' $HOME/.pryzm/config/config.toml
```
## İndexer Kapat
```
sed -i -e 's|^indexer *=.*|indexer = null|' $HOME/.pryzm/config/config.toml
```
## Port Değiştir
> ### Port=316
```
sed -i -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:31658\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:31657\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:31660\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:31656\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":31666\"%" $HOME/.pryzm/config/config.toml
sed -i -e "s%^address = \"tcp://localhost:1317\"%address = \"tcp://localhost:31617\"%; s%^address = \":8080\"%address = \":31680\"%; s%^address = \"localhost:9090\"%address = \"localhost:31690\"%; s%^address = \"localhost:9091\"%address = \"localhost:31691\"%; s%:8545%:31645%; s%:8546%:31646%; s%:6065%:31665%" $HOME/.pryzm/config/app.toml
```
## Min Gas Price Ayarla
```

```
## Prometheus Aktif Et
```
sed -i -e s/prometheus = false/prometheus = true/ $HOME/.pryzm/config/config.toml
```
## Zincir Verilerini Sıfırla
```
pryzmd tendermint unsafe-reset-all --keep-addr-book --home $HOME/.pryzm --keep-addr-book
```
<h1 align=center> Durum Sorgulama ve Kontrol </h1>
 
## Senkronizasyon Durumu
```
pryzmd status 2>&1 | jq .SyncInfo
```
## Validatör Durumu
```
pryzmd status 2>&1 | jq .ValidatorInfo
```
## Node Durumu
```
pryzmd status 2>&1 | jq .NodeInfo
```
## Validatör Key Kontrol
```
[[ $(pryzmd q staking validator $(pryzmd keys show wallet --bech val -a) -oj | jq -r .consensus_pubkey.key) = $(pryzmd status | jq -r .ValidatorInfo.PubKey.value) ]] && echo -e "\n\e[1m\e[32mTrue\e[0m\n" || echo -e "\n\e[1m\e[31mFalse\e[0m\n"
```
## TX Sorgulama
```
pryzmd query tx <TX_ID>
```
## Peer Adresini Öğren
```
echo $(pryzmd tendermint show-node-id)@$(curl -s ifconfig.me):$(cat $HOME/.pryzm/config/config.toml | sed -n '/Address to listen for incoming connection/{n;p;}' | sed 's/.*://; s/".*//')
```
## Bağlı Peerleri Öğren
```
curl -sS http://localhost:31657/net_info | jq -r ".result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr)"" | awk -F ":" "{print $1":"$NF}"
```
<h1 align=center> Service Yönetimi </h1>
 
Servisi Etkinleştir
```
sudo systemctl enable pryzmd
```
Servisi Devre Dışı Bırak
```
sudo systemctl disable pryzmd
```
Servisi Başlat
```
sudo systemctl start pryzmd
```
Servisi Durdur
```
sudo systemctl stop pryzmd
```
Servisi Yeniden Başlat
```
sudo systemctl restart pryzmd
```
Servis Durumunu Kontrol Et
```
sudo systemctl status pryzmd
```
Servis Loglarını Kontrol Et
```
sudo journalctl -u pryzmd -f --no-hostname -o cat
```
<h1 align=center> Node Silmek </h1>
 
```
sudo systemctl stop pryzmd && sudo systemctl disable pryzmd && sudo rm /etc/systemd/system/pryzmd.service && sudo systemctl daemon-reload && rm -rf $HOME/.pryzm && rm -rf $HOME/pryzm && sudo rm -rf $(which pryzmd)
```
