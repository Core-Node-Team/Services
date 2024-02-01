
![artela](https://github.com/molla202/Artela/assets/91562185/a7922117-442e-4bbf-b56a-1d11e09670f7)

## Linkler:
 * [Topluluk kanalımız](https://t.me/corenodechat)
 * [Topluluk Twitter](https://twitter.com/corenodeHQ)
 * [Artela Resmi Websitesi](https://artela.network/)
 * [Artela Resmi Twitter](https://twitter.com/Artela_Network)
 * [Artela Resmi Discord](https://discord.gg/TzmnmuCU)
 * [Artela Dökümantasyon](https://docs.artela.network/develop/node/run-full-node)
 * [Artela Tüm Linkler](https://linktr.ee/artela_network)
 * [Artela EXPLORER](https://test.explorer.ist/artela/staking)




### Not: faucet almak için cüzdan olusturduktan sonra altaki kodla adresinizi öğrenin Address (EIP-55): adresiniz burda yazıor
```
artelad debug addr art-ile-başlayancüzdan-adresinizi-yazınız
```



### Sistem Gereksinimleri

| Bileşenler | Minimum Gereksinimler | 
| ------------ | ------------ |
| ✔️CPU |	4 vcpu|
| ✔️RAM	| 8 GB |
| ✔️Storage	| 200+ GB SSD |
| ✔️UBUNTU | 20or22 |


### Update ve gereklilikler
```
apt update && apt upgrade -y
apt install curl iptables build-essential git wget jq make gcc nano tmux htop lz4 nvme-cli pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev -y
```
### Go kurulum
```
cd $HOME
wget https://go.dev/dl/go1.20.4.linux-amd64.tar.gz
rm -rf /usr/local/go && tar -C /usr/local -xzf go1.20.4.linux-amd64.tar.gz
echo 'export GOROOT=/usr/local/go' >> $HOME/.bash_profile
echo 'export GOPATH=$HOME/go' >> $HOME/.bash_profile
echo 'export GO111MODULE=on' >> $HOME/.bash_profile
echo 'export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin' >> $HOME/.bash_profile && source $HOME/.bash_profile
rm -rf go1.20.4.linux-amd64.tar.gz
go version
```
### Varyasyonlar (moniker ve cüzdan adınızı değiştiriniz)
```
echo "export WALLET="wallet"" >> $HOME/.bash_profile
echo "export MONIKER="test"" >> $HOME/.bash_profile
echo "export ARTELA_CHAIN_ID="artela_11822-1"" >> $HOME/.bash_profile
echo "export ARTELA_PORT="45"" >> $HOME/.bash_profile
source $HOME/.bash_profile

cd $HOME
rm -rf artela
git clone https://github.com/artela-network/artela
cd artela
git checkout v0.4.7-rc6
make install

artelad config chain-id artela_11822-1
artelad init "$MONIKER" --chain-id artela_11822-1

wget -qO $HOME/.artelad/config/genesis.json https://raw.githubusercontent.com/Core-Node-Team/scripts/main/artela/genesis.json
wget -qO $HOME/.artelad/config/addrbook.json https://raw.githubusercontent.com/Core-Node-Team/scripts/main/artela/addrbook.json


SEEDS="8d0c626443a970034dc12df960ae1b1012ccd96a@artela-testnet-seed.itrocket.net:30656"
PEERS="5c9b1bc492aad27a0197a6d3ea3ec9296504e6fd@artela-testnet-peer.itrocket.net:30656,894f5dbc0905b04cbab24087d8f205067e97cbfe@194.247.12.103:26656,a27fec04636e9c67444e3d2dc57bfd389cfe69ca@5.78.113.161:45656,88b0fd5f9eee3b77509688fb7ff11667c6625dfb@185.197.194.106:26656,ed6e38e7854f38ae64cb38474a22b07dd65c13db@84.247.133.43:45656,7d326994eca02cff23b7957a04bb4eb6ab3913ca@84.247.169.242:28656,b233154ef8973d6570bd2d8a6dd85102fc3b491e@65.109.133.245:26656,01a5e97354d2dc526d2b5aa54f5b90f4105e7a96@84.247.160.34:45656,d8169d0479b5825c53b7d77e3221ff3f2518a02a@50.114.185.80:26656,6ee26cda128c77aa09ee3185b75c46dd9f110d72@37.60.233.157:45656,b87df5cd28aa262b100eb85d2f78024b17e3e53b@65.109.49.56:30656"
sed -i -e "s/^seeds *=.*/seeds = \"$SEEDS\"/; s/^persistent_peers *=.*/persistent_peers = \"$PEERS\"/" $HOME/.artelad/config/config.toml

sed -E 's/^pool-size[[:space:]]*=[[:space:]]*[0-9]+$/apply-pool-size = 10\nquery-pool-size = 30/' ~/.artelad/config/app.toml > ~/.artelad/config/temp.app.toml && mv ~/.artelad/config/temp.app.toml ~/.artelad/config/app.toml

sed -i 's|^pruning *=.*|pruning = "custom"|g' $HOME/.artelad/config/app.toml
sed -i 's|^pruning-keep-recent  *=.*|pruning-keep-recent = "100"|g' $HOME/.artelad/config/app.toml
sed -i 's|^pruning-interval *=.*|pruning-interval = "10"|g' $HOME/.artelad/config/app.toml
sed -i 's|^snapshot-interval *=.*|snapshot-interval = 0|g' $HOME/.artelad/config/app.toml

sed -i 's|^minimum-gas-prices *=.*|minimum-gas-prices = "0.025art"|g' $HOME/.artelad/config/app.toml
sed -i 's|^prometheus *=.*|prometheus = true|' $HOME/.artelad/config/config.toml
sed -i 's|^indexer *=.*|indexer = "null"|' $HOME/.artelad/config/config.toml

# set custom ports in app.toml
sed -i.bak -e "s%:1317%:${ARTELA_PORT}317%g;
s%:8080%:${ARTELA_PORT}080%g;
s%:9090%:${ARTELA_PORT}090%g;
s%:9091%:${ARTELA_PORT}091%g;
s%:8545%:${ARTELA_PORT}545%g;
s%:8546%:${ARTELA_PORT}546%g;
s%:6065%:${ARTELA_PORT}065%g" $HOME/.artelad/config/app.toml

# set custom ports in config.toml file
sed -i.bak -e "s%:26658%:${ARTELA_PORT}658%g;
s%:26657%:${ARTELA_PORT}657%g;
s%:6060%:${ARTELA_PORT}060%g;
s%:26656%:${ARTELA_PORT}656%g;
s%^external_address = \"\"%external_address = \"$(wget -qO- eth0.me):${ARTELA_PORT}656\"%;
s%:26660%:${ARTELA_PORT}660%g" $HOME/.artelad/config/config.toml

sudo tee /etc/systemd/system/artelad.service > /dev/null << EOF
[Unit]
Description=artela node
After=network-online.target
[Service]
User=$USER
ExecStart=$(which artelad) start
Restart=on-failure
RestartSec=10
LimitNOFILE=10000
[Install]
WantedBy=multi-user.target
EOF
```

### snap
```
artelad tendermint unsafe-reset-all --home $HOME/.artelad --keep-addr-book

curl -L http://37.120.189.81/artela_testnet/artela_snap.tar.lz4 | tar -I lz4 -xf - -C $HOME/.artelad
```
### Başlatalım
```
sudo systemctl daemon-reload
sudo systemctl enable artelad
sudo systemctl start artelad
```
### Log
```
sudo journalctl -fu artelad -o cat
```

### Cüzdan olusturma

artelad keys add cüzdan-adı

### validator olusturma
Not: 2 defa faucet alıp yapın.moniker ve cüzdan adınızı yazınız
```
artelad tx staking create-validator \
--amount 950000uart \
--from cüzdan-adınız \
--commission-rate 0.1 \
--commission-max-rate 0.2 \
--commission-max-change-rate 0.01 \
--min-self-delegation 1 \
--pubkey $(artelad tendermint show-validator) \
--moniker "moniker-adınız" \
--identity="" \
--website="" \
--details="Mustafa Kemal ATATÜRK❤️" \
--chain-id artela_11822-1 \
--gas auto \
--gas-adjustment 1.4 \
--gas-prices 0.055uart \
--node http://localhost:45657 \
-y
```
### Delege
Not: cüzdan-adınız kısımlarını değiştiriniz.
```
artelad tx staking delegate $(artelad keys show cüzdan-adınız --bech val -a) 1.5art --from cüzdan-adınız --chain-id artela_11822-1 --gas-adjustment 1.4 --gas auto --gas-prices 0.025uart --node http://localhost:45657 -y
```

