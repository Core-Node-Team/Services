 
#
<h1 align=center> Cüzdan Yönetimi </h1>
 
## Cüzdan Oluştur
```
althea keys add wallet
```
## Cüzdan Recover Et
```
althea keys add wallet --recover
```
## Cüzdanları Listele
```
althea keys list
```
## Cüzdan Sil
```
althea keys delete wallet
```
## Cüzdan Bakiyesini Sorgula
```
althea q bank balances $(althea keys show wallet -a)
```
#
<h1 align=center> Validatör Yönetimi </h1>
 
## Validatör Oluştur
```
althea tx staking create-validator \
--amount 1000000aalthea \
--pubkey $(althea tendermint show-validator) \
--moniker "MONİKER_İSMİNİZ" \
--identity "KEYBASE.İO_İD" \
--details "DETAYLAR" \
--website "WEBSİTE_LİNKİNİZ" \
--chain-id althea_417834-3 \
--commission-rate 0.05 \
--commission-max-rate 0.20 \
--commission-max-change-rate 0.01 \
--min-self-delegation 1 \
--from wallet \
--gas-adjustment 1.5 \
--gas auto \
--gas-prices 0aalthea \
-y
```
## Validatörü Düzenle
```
althea tx staking edit-validator \
--new-moniker "MONİKER_İSMİNİZ" \
--identity "KEYBASE.İO_İD" \
--details "DETAYLAR" \
--website "WEBSİTE_LİNKİNİZ" \
--chain-id althea_417834-3 \
--commission-rate 0.05 \
--from wallet \
--gas-adjustment 1.5 \
--gas auto \
--gas-prices 0aalthea \
-y
```
## Validatör Detayları
```
althea q staking validator $(althea keys show wallet --bech val -a)
```
## Validatör Unjail
```
althea tx slashing unjail --from wallet --chain-id althea_417834-3 --gas-adjustment 1.5 --gas auto --gas-prices 0aalthea -y
```
## Jail Olma Sebebi
```
althea query slashing signing-info $(althea tendermint show-validator)
```
## Tüm Aktif Validatörleri Listele
```
althea q staking validators -oj --limit=3000 | jq '.validators[] | select(.status==BOND_STATUS_BONDED)' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) +  t  + .description.moniker' | sort -gr | nl
```
## Tüm İnaktif Validatörleri Listele
```
althea q staking validators -oj --limit=3000 | jq '.validators[] | select(.status==BOND_STATUS_UNBONDED)' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) +  t  + .description.moniker' | sort -gr | nl
```
<h1 align=center> Token </h1>
 
## Token Gönder
```
althea tx bank send wallet <HEDEF_CÜZDAN_ADRESİ> 1000000aalthea --from wallet --chain-id althea_417834-3 --gas-adjustment 1.5 --gas auto --gas-prices 0aalthea -y
```
## Delegate
```
althea tx staking delegate <VALOPER_ADRESİ> 1000000aalthea --from wallet --chain-id althea_417834-3 --gas-adjustment 1.5 --gas auto --gas-prices 0aalthea -y
```
## Kendi Validatörüne Delegate
```
althea tx staking delegate $(althea keys show wallet --bech val -a) 1000000aalthea --from wallet --chain-id althea_417834-3 --gas-adjustment 1.5 --gas auto ---gas-prices 0aalthea -y
```
## Redelegate
```
althea tx staking redelegate <İLK_VALOPER_ADRESİ> <HEDEF_VALOPER_ADRESİ> 1000000aalthea --from wallet --chain-id althea_417834-3 --gas-adjustment 1.5 --gas auto --gas-prices 0aalthea -y
```
## Kendi Validatöründen Başka Validatöre Redelegate
```
althea tx staking redelegate $(althea keys show wallet --bech val -a) <VALOPER_ADRESİ> 1000000aalthea --from wallet --chain-id althea_417834-3 --gas-adjustment 1.5 --gas auto --gas-prices 0aalthea -y
```
## Unbond
```
althea tx staking unbond $(althea keys show wallet --bech val -a) 1000000aalthea --from wallet --chain-id althea_417834-3 --gas-adjustment 1.5 --gas auto --gas-prices 0aalthea -y
```
## Tüm Validatörlerden Komisyon ve Ödülleri Çekme
```
althea tx distribution withdraw-all-rewards --commission --from wallet --chain-id althea_417834-3 --gas-adjustment 1.5 --gas auto --gas-prices 0aalthea -y
```
## Kendi Validatörünüze Ait Komisyon ve Ödülleri Çekme
```
althea tx distribution withdraw-rewards $(althea keys show wallet --bech val -a) --commission --from wallet --chain-id althea_417834-3 --gas-adjustment 1.5 --gas auto --gas-prices 0aalthea -y
```
<h1 align=center> Yönetim </h1>
 
## Tüm Oylamaları Görüntüle
```
althea query gov proposals
```
## Oylama Detaylarını Görüntüle
```
althea query gov proposal <ID>
```
## Evet Oyu Ver
```
althea tx gov vote <ID> yes --from wallet --chain-id althea_417834-3 --gas-adjustment 1.5 --gas auto --gas-prices 0aalthea -y
```
## Hayır Oyu Ver
```
althea tx gov vote <ID> no --from wallet --chain-id althea_417834-3 --gas-adjustment 1.5 --gas auto --gas-prices 0aalthea -y
```
## Çekimser Oyu Ver
```
althea tx gov vote <ID> abstain --from wallet --chain-id althea_417834-3 --gas-adjustment 1.5 --gas auto --gas-prices 0aalthea -y
```
## Hayır Oyu ve Veto Et
```
althea tx gov vote <ID> no_with_veto --from wallet --chain-id althea_417834-3 --gas-adjustment 1.5 --gas auto --gas-prices 0aalthea -y
```
<h1 align=center> Yapılandırma Ayarları </h1>
 
 ## Pruning
```
sed -i \
  -e 's|^pruning *=.*|pruning = "custom"|' \
  -e 's|^pruning-keep-recent *=.*|pruning-keep-recent = "100"|' \
  -e 's|^pruning-keep-every *=.*|pruning-keep-every = "0"|' \
  -e 's|^pruning-interval *=.*|pruning-interval = "10"|' \
  $HOME/.althea/config/app.toml
```
## İndexer Aç
```
sed -i -e 's|^indexer *=.*|indexer = kv|' $HOME/.althea/config/config.toml
```
## İndexer Kapat
```
sed -i -e 's|^indexer *=.*|indexer = null|' $HOME/.althea/config/config.toml
```
## Port Değiştir
> ### Port=315
```
sed -i -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:31558\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:31557\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:31560\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:31556\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":31566\"%" $HOME/.althea/config/config.toml
sed -i -e "s%^address = \"tcp://localhost:1317\"%address = \"tcp://localhost:31517\"%; s%^address = \":8080\"%address = \":31580\"%; s%^address = \"localhost:9090\"%address = \"localhost:31590\"%; s%^address = \"localhost:9091\"%address = \"localhost:31591\"%; s%:8545%:31545%; s%:8546%:31546%; s%:6065%:31565%" $HOME/.althea/config/app.toml
```
## Min Gas Price Ayarla
```

```
## Prometheus Aktif Et
```
sed -i -e s/prometheus = false/prometheus = true/ $HOME/.althea/config/config.toml
```
## Zincir Verilerini Sıfırla
```
althea tendermint unsafe-reset-all --keep-addr-book --home $HOME/.althea --keep-addr-book
```
<h1 align=center> Durum Sorgulama ve Kontrol </h1>
 
## Senkronizasyon Durumu
```
althea status 2>&1 | jq .SyncInfo
```
## Validatör Durumu
```
althea status 2>&1 | jq .ValidatorInfo
```
## Node Durumu
```
althea status 2>&1 | jq .NodeInfo
```
## Validatör Key Kontrol
```
[[ $(althea q staking validator $(althea keys show wallet --bech val -a) -oj | jq -r .consensus_pubkey.key) = $(althea status | jq -r .ValidatorInfo.PubKey.value) ]] && echo -e "\n\e[1m\e[32mTrue\e[0m\n" || echo -e "\n\e[1m\e[31mFalse\e[0m\n"
```
## TX Sorgulama
```
althea query tx <TX_ID>
```
## Peer Adresini Öğren
```
echo $(althea tendermint show-node-id)@$(curl -s ifconfig.me):$(cat $HOME/.althea/config/config.toml | sed -n '/Address to listen for incoming connection/{n;p;}' | sed 's/.*://; s/".*//')
```
## Bağlı Peerleri Öğren
```
curl -sS http://localhost:31557/net_info | jq -r ".result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr)"" | awk -F ":" "{print $1":"$NF}"
```
<h1 align=center> Service Yönetimi </h1>
 
Servisi Etkinleştir
```
sudo systemctl enable althea
```
Servisi Devre Dışı Bırak
```
sudo systemctl disable althea
```
Servisi Başlat
```
sudo systemctl start althea
```
Servisi Durdur
```
sudo systemctl stop althea
```
Servisi Yeniden Başlat
```
sudo systemctl restart althea
```
Servis Durumunu Kontrol Et
```
sudo systemctl status althea
```
Servis Loglarını Kontrol Et
```
sudo journalctl -u althea -f --no-hostname -o cat
```
<h1 align=center> Node Silmek </h1>
 
```
sudo systemctl stop althea && sudo systemctl disable althea && sudo rm /etc/systemd/system/althea.service && sudo systemctl daemon-reload && rm -rf $HOME/.althea && rm -rf $HOME/althea-chain && sudo rm -rf $(which althea)
```
