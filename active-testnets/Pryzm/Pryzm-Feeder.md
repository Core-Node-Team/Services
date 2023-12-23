#

## Timescaledb Kurulumu
```
apt install gnupg postgresql-common apt-transport-https lsb-release wget

/usr/share/postgresql-common/pgdg/apt.postgresql.org.sh

echo "deb https://packagecloud.io/timescale/timescaledb/ubuntu/ $(lsb_release -c -s) main" | sudo tee /etc/apt/sources.list.d/timescaledb.list
```
##### ubuntu 20.04
```
wget --quiet -O - https://packagecloud.io/timescale/timescaledb/gpgkey | sudo apt-key add -
```
##### ubuntu 22.04
```
wget --quiet -O - https://packagecloud.io/timescale/timescaledb/gpgkey | sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/timescaledb.gpg
```
```
apt update
apt install timescaledb-2-postgresql-14
```
## postgresql
```
apt-get update
apt-get install postgresql-client
```
```
systemctl restart postgresql
sudo -u postgres psql
```
##### Şifre belirle, kaydet
```
\password postgres
```
##### Postgresden çıkış
```
\q
```
##### Postgres bağlan
```
psql -U postgres -h localhost
```
##### Database oluştur
```
CREATE database pryzmfeeder;
# kontrol
\c pryzmfeeder
\dx
```
###### Çıkış
```
\q
```
## Feeder Cüzdan
* Kaydet sakla
```
pryzmd keys add feeder
```
##### Faucetten token al
  
## Pryzm-Feeder Binary
```
wget https://storage.googleapis.com/pryzm-zone/feeder/pryzm-feeder.tgz
tar -xzf pryzm-feeder.tgz
mkdir -p feeder/app
cd feeder/app
wget https://storage.googleapis.com/pryzm-zone/feeder/config.yaml
wget https://storage.googleapis.com/pryzm-zone/feeder/init.sql
```
## Yapılandırma dosyası
### Değişkenleri ayarla
```
Adres="feeder adres"
Mnemonic="feeder mnemonic"
Validator="valoper adresi"
Password="postgres şifresi"
CustomPort="316"
GasPrice="0.015upryzm"
```
* sırayla gir
```
sed -i 's/name: "pryzm-feeder"/name: "pryzmfeeder"/g' config.yaml
sed -i 's/gasPrice: "[^"]*"/gasPrice: "'"$GasPrice"'"/g' config.yaml
sed -i 's/feeder: ""/feeder: "'"$Adres"'"/g' config.yaml
sed -i 's/feederMnemonic: ""/feederMnemonic: "'"$Mnemonic"'"/g' config.yaml
sed -i 's/password: ".*"/password: "'"$Password"'"/g' config.yaml
sed -i 's/validator: ""/validator: "'"$Validator"'"/g' config.yaml
sed -i 's/rpcUrl: "http:\/\/localhost:[^"]*"/rpcUrl: "http:\/\/localhost:'"$CustomPort"'57"/g' config.yaml
sed -i 's/grpcWebUrl: "http:\/\/localhost:[^"]*"/grpcWebUrl: "http:\/\/localhost:'"$CustomPort"'91"/g' config.yaml
sed -i 's/wsUrl: "ws:\/\/localhost:[^"]*"/wsUrl: "ws:\/\/localhost:'"$CustomPort"'57"/g' config.yaml
sed -i '0,/lcdUrl: "http:\/\/localhost:[^"]*"/ s/lcdUrl: "http:\/\/localhost:[^"]*"/lcdUrl: "http:\/\/localhost:'"$CustomPort"'17"/' config.yaml
```


## İndexer Aç
```
sed -i 's|^indexer *=.*|indexer = "kv"|' $HOME/.pryzm/config/config.toml
```
## Cüzdanları bağla
```
pryzmd tx oracle delegate-feed-consent $Adres --from wallet --gas-prices 0.015upryzm
```
## Node.js Kur
```
curl -sL https://deb.nodesource.com/setup_18.x | sudo bash -
```
```
sudo apt-get install -y nodejs
node -v
```
## Başlat
```
screen -S feeder
cd $HOME/feeder
node ./lib/vote.js app/config.yaml
```









