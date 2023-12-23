#
```
apt install gnupg postgresql-common apt-transport-https lsb-release wget

/usr/share/postgresql-common/pgdg/apt.postgresql.org.sh

echo "deb https://packagecloud.io/timescale/timescaledb/ubuntu/ $(lsb_release -c -s) main" | sudo tee /etc/apt/sources.list.d/timescaledb.list
```
* ubuntu 20.04
```
wget --quiet -O - https://packagecloud.io/timescale/timescaledb/gpgkey | sudo apt-key add -
```
* ubuntu 22.04
```
wget --quiet -O - https://packagecloud.io/timescale/timescaledb/gpgkey | sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/timescaledb.gpg
```
```
apt update
apt install timescaledb-2-postgresql-14

apt-get update
apt-get install postgresql-client
```
```
systemctl restart postgresql
sudo -u postgres psql
```
```
\password postgres
```
* exit
```
\q
```

```
psql -U postgres -h localhost
\c pryzmfeeder
\dx
```
```
\q
```
## Feeder Cüzdan
* kaydet
```
pryzm keys add feeder
```
* faucet
  
## Pryzm-Feeder Binary
```
wget https://storage.googleapis.com/pryzm-zone/feeder/pryzm-feeder.tgz
tar -xzf pryzm-feeder.tgz
mkdir -p feeder/app
cd feeder/app
wget https://storage.googleapis.com/pryzm-zone/feeder/config.yaml
wget https://storage.googleapis.com/pryzm-zone/feeder/init.sql
```
```
nano config.yaml
```







