 
#
<h1 align=center> Cüzdan Yönetimi </h1>
 
## Cüzdan Oluştur
```
ojod keys add wallet
```
## Cüzdan Recover Et
```
ojod keys add wallet --recover
```
## Cüzdanları Listele
```
ojod keys list
```
## Cüzdan Sil
```
ojod keys delete wallet
```
## Cüzdan Bakiyesini Sorgula
```
ojod q bank balances $(ojod keys show wallet -a)
```
#
<h1 align=center> Validatör Yönetimi </h1>
 
## Validatör Oluştur
```
ojod tx staking create-validator \
--amount 1000000uojo \
--pubkey $(ojod tendermint show-validator) \
--moniker "MONİKER_İSMİNİZ" \
--identity "KEYBASE.İO_İD" \
--details "DETAYLAR" \
--website "WEBSİTE_LİNKİNİZ" \
--chain-id ojo-devnet \
--commission-rate 0.05 \
--commission-max-rate 0.20 \
--commission-max-change-rate 0.01 \
--min-self-delegation 1 \
--from wallet \
--gas-adjustment 1.5 \
--gas auto \
--fees 100uojo \
-y
```
## Validatörü Düzenle
```
ojod tx staking edit-validator \
--new-moniker "MONİKER_İSMİNİZ" \
--identity "KEYBASE.İO_İD" \
--details "DETAYLAR" \
--website "WEBSİTE_LİNKİNİZ" \
--chain-id ojo-devnet \
--commission-rate 0.05 \
--from wallet \
--gas-adjustment 1.5 \
--gas auto \
--fees 100uojo \
-y
```
## Validatör Detayları
```
ojod q staking validator $(ojod keys show wallet --bech val -a)
```
## Validatör Unjail
```
ojod tx slashing unjail --from wallet --chain-id ojo-devnet --gas-adjustment 1.5 --gas auto --fees 100uojo -y
```
## Jail Olma Sebebi
```
ojod query slashing signing-info $(ojod tendermint show-validator)
```
## Tüm Aktif Validatörleri Listele
```
ojod q staking validators -oj --limit=3000 | jq '.validators[] | select(.status==BOND_STATUS_BONDED)' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) +  t  + .description.moniker' | sort -gr | nl
```
## Tüm İnaktif Validatörleri Listele
```
ojod q staking validators -oj --limit=3000 | jq '.validators[] | select(.status==BOND_STATUS_UNBONDED)' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) +  t  + .description.moniker' | sort -gr | nl
```
<h1 align=center> Token </h1>
 
## Token Gönder
```
ojod tx bank send wallet <HEDEF_CÜZDAN_ADRESİ> 1000000uojo --from wallet --chain-id ojo-devnet --gas-adjustment 1.5 --gas auto --fees 100uojo -y
```
## Delegate
```
ojod tx staking delegate <VALOPER_ADRESİ> 1000000uojo --from wallet --chain-id ojo-devnet --gas-adjustment 1.5 --gas auto --fees 100uojo -y
```
## Kendi Validatörüne Delegate
```
ojod tx staking delegate $(ojod keys show wallet --bech val -a) 1000000uojo --from wallet --chain-id ojo-devnet --gas-adjustment 1.5 --gas auto --fees 100uojo -y
```
## Redelegate
```
ojod tx staking redelegate <İLK_VALOPER_ADRESİ> <HEDEF_VALOPER_ADRESİ> 1000000uojo --from wallet --chain-id ojo-devnet --gas-adjustment 1.5 --gas auto --fees 100uojo -y
```
## Kendi Validatöründen Başka Validatöre Redelegate
```
ojod tx staking redelegate $(ojod keys show wallet --bech val -a) <VALOPER_ADRESİ> 1000000uojo --from wallet --chain-id ojo-devnet --gas-adjustment 1.5 --gas auto --fees 100uojo -y
```
## Unbond
```
ojod tx staking unbond $(ojod keys show wallet --bech val -a) 1000000uojo --from wallet --chain-id ojo-devnet --gas-adjustment 1.5 --gas auto --fees 100uojo -y
```
## Tüm Validatörlerden Komisyon ve Ödülleri Çekme
```
ojod tx distribution withdraw-all-rewards --commission --from wallet --chain-id ojo-devnet --gas-adjustment 1.5 --gas auto --fees 100uojo -y
```
## Kendi Validatörünüze Ait Komisyon ve Ödülleri Çekme
```
ojod tx distribution withdraw-rewards $(ojod keys show wallet --bech val -a) --commission --from wallet --chain-id ojo-devnet --gas-adjustment 1.5 --gas auto --fees 100uojo -y
```
<h1 align=center> Yönetim </h1>
 
## Tüm Oylamaları Görüntüle
```
ojod query gov proposals
```
## Oylama Detaylarını Görüntüle
```
ojod query gov proposal <ID>
```
## Evet Oyu Ver
```
ojod tx gov vote <ID> yes --from wallet --chain-id ojo-devnet --gas-adjustment 1.5 --gas auto --fees 100uojo -y
```
## Hayır Oyu Ver
```
ojod tx gov vote <ID> no --from wallet --chain-id ojo-devnet --gas-adjustment 1.5 --gas auto --fees 100uojo -y
```
## Çekimser Oyu Ver
```
ojod tx gov vote <ID> abstain --from wallet --chain-id ojo-devnet --gas-adjustment 1.5 --gas auto --fees 100uojo -y
```
## Hayır Oyu ve Veto Et
```
ojod tx gov vote <ID> no_with_veto --from wallet --chain-id ojo-devnet --gas-adjustment 1.5 --gas auto --fees 100uojo -y
```
<h1 align=center> Yapılandırma Ayarları </h1>
 
 ## Pruning
```
sed -i \
  -e 's|^pruning *=.*|pruning = "custom"|' \
  -e 's|^pruning-keep-recent *=.*|pruning-keep-recent = "100"|' \
  -e 's|^pruning-keep-every *=.*|pruning-keep-every = "0"|' \
  -e 's|^pruning-interval *=.*|pruning-interval = "10"|' \
  $HOME/.ojod/config/app.toml
```
## İndexer Aç
```
sed -i -e 's|^indexer *=.*|indexer = kv|' $HOME/.ojod/config/config.toml
```
## İndexer Kapat
```
sed -i -e 's|^indexer *=.*|indexer = null|' $HOME/.ojod/config/config.toml
```
## Port Değiştir
> ### Port=312
```
sed -i -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:31258\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:31257\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:31260\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:31256\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":31266\"%" $HOME/.ojod/config/config.toml
sed -i -e "s%^address = \"tcp://localhost:1317\"%address = \"tcp://localhost:31217\"%; s%^address = \":8080\"%address = \":31280\"%; s%^address = \"localhost:9090\"%address = \"localhost:31290\"%; s%^address = \"localhost:9091\"%address = \"localhost:31291\"%; s%:8545%:31245%; s%:8546%:31246%; s%:6065%:31265%" $HOME/.ojod/config/app.toml
```
## Min Gas Price Ayarla
```

```
## Prometheus Aktif Et
```
sed -i -e s/prometheus = false/prometheus = true/ $HOME/.ojod/config/config.toml
```
## Zincir Verilerini Sıfırla
```
ojod tendermint unsafe-reset-all --keep-addr-book --home $HOME/.ojod --keep-addr-book
```
<h1 align=center> Durum Sorgulama ve Kontrol </h1>
 
## Senkronizasyon Durumu
```
ojod status 2>&1 | jq .SyncInfo
```
## Validatör Durumu
```
ojod status 2>&1 | jq .ValidatorInfo
```
## Node Durumu
```
ojod status 2>&1 | jq .NodeInfo
```
## Validatör Key Kontrol
```
[[ $(ojod q staking validator $(ojod keys show wallet --bech val -a) -oj | jq -r .consensus_pubkey.key) = $(ojod status | jq -r .ValidatorInfo.PubKey.value) ]] && echo -e "\n\e[1m\e[32mTrue\e[0m\n" || echo -e "\n\e[1m\e[31mFalse\e[0m\n"
```
## TX Sorgulama
```
ojod query tx <TX_ID>
```
## Peer Adresini Öğren
```
echo $(ojod tendermint show-node-id)@$(curl -s ifconfig.me):$(cat $HOME/.ojod/config/config.toml | sed -n '/Address to listen for incoming connection/{n;p;}' | sed 's/.*://; s/".*//')
```
## Bağlı Peerleri Öğren
```
curl -sS http://localhost:31257/net_info | jq -r ".result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr)"" | awk -F ":" "{print $1":"$NF}"
```
<h1 align=center> Service Yönetimi </h1>
 
## Servisi Etkinleştir
```
sudo systemctl enable ojod
```
## Servisi Devre Dışı Bırak
```
sudo systemctl disable ojod
```
## Servisi Başlat
```
sudo systemctl start ojod
```
## Servisi Durdur
```
sudo systemctl stop ojod
```
## Servisi Yeniden Başlat
```
sudo systemctl restart ojod
```
## Servis Durumunu Kontrol Et
```
sudo systemctl status ojod
```
## Servis Loglarını Kontrol Et
```
sudo journalctl -u ojod -f --no-hostname -o cat
```
<h1 align=center> Node Silmek </h1>
 
```
sudo systemctl stop ojod && sudo systemctl disable ojod && sudo rm /etc/systemd/system/ojod.service && sudo systemctl daemon-reload && rm -rf $HOME/.ojod && rm -rf $HOME/ojo && sudo rm -rf $(which ojod)
```
