# Manuel Installation

![arkeo](https://github.com/Core-Node-Team/Gitbook/assets/108215275/01f3074a-af3d-4d44-a47d-800cdbbb0feb)



## Prepare Server

```bash
sudo apt-get update && sudo apt-get upgrade -y

sudo apt install curl tar wget tmux htop net-tools clang pkg-config libssl-dev jq build-essential git screen make ncdu -y

cd $HOME
wget https://go.dev/dl/go1.20.4.linux-amd64.tar.gz
rm -rf /usr/local/go && tar -C /usr/local -xzf go1.20.4.linux-amd64.tar.gz
echo 'export GOROOT=/usr/local/go' >> $HOME/.bash_profile
echo 'export GOPATH=$HOME/go' >> $HOME/.bash_profile
echo 'export GO111MODULE=on' >> $HOME/.bash_profile
echo 'export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin' >> $HOME/.bash_profile && source $HOME/.bash_profile
rm -rf go1.20.4.linux-amd64.tar.gz
```

## Install Binary
```bash
wget http://snapshot.corenode.info/arkeo_testnet/arkeod
chmod +x arkeod
mv arkeod $HOME/go/bin/
```

## Initalize
```bash
arkeod config chain-id arkeo
arkeod config keyring-backend test
arkeod config node tcp://localhost:31457
arkeod init $MONIKER --chain-id arkeo
```

## Config
```bash
curl -Ls https://raw.githubusercontent.com/Core-Node-Team/Testnet-TR/main/Arkeo/addrbook.json > $HOME/$DirectName/config/addrbook.json

curl -Ls https://raw.githubusercontent.com/Core-Node-Team/Testnet-TR/main/Arkeo/genesis.json > $HOME/$DirectName/config/genesis.json

peers="8c2d799bcc4fbf44ef34bbd2631db5c3f4619e41@213.239.207.175:60656,e46d22832e1746d099c7a0e329cee8d904337718@65.109.48.181:32656,0c96f2b20f856186dbff5dd2e85640aaae8a6034@5.181.190.76:22856,cb9401d70e1bd59e3ed279942ce026dae82aca1f@65.109.33.48:27656,465600bad30995e46124d5ec23021f4845be2ece@38.242.210.137:26656,32ec9022a9565e490bf28ce6550739156fd1e41e@81.0.221.203:26656,374facfe63ab4c786d484c2d7d614063190590b7@88.99.213.25:38656,1eaeb5b9cb2cc1ae5a14d5b87d65fef89998b467@65.108.141.109:17656,9c0df85008e400b440232fdb7470c593fa07609a@154.53.56.176:30656,65c95f70cf0ca8948f6ff59e83b22df3f8484edf@65.108.226.183:22856"
seeds="0e1000e88125698264454a884812746c2eb4807@seeds.lavenderfive.com:22856"
sed -i -e 's|^seeds *=.*|seeds = "'$seeds'"|; s|^persistent_peers *=.*|persistent_peers = "'$peers'"|' $HOME/.arkeo/config/config.toml

sed -i 's|^minimum-gas-prices *=.*|minimum-gas-prices = "0.0001uarkeo"|g' $HOME/.arkeo/config/app.toml
sed -i 's|^prometheus *=.*|prometheus = true|' $HOME/.arkeo/config/config.toml

sed -i -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:31458\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:31457\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:31460\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:31456\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":31466\"%" $HOME/.arkeo/config/config.toml
sed -i -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:31417\"%; s%^address = \":8080\"%address = \":31480\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:31490\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:31491\"%; s%:8545%:31445%; s%:8546%:31446%; s%:6065%:31465%" $HOME/.arkeo/config/app.toml
sed -i -e "s%^address = \"tcp://localhost:1317\"%address = \"tcp://localhost:31417\"%; s%^address = \":8080\"%address = \":31480\"%; s%^address = \"localhost:9090\"%address = \"localhost:31490\"%; s%^address = \"localhost:9091\"%address = \"localhost:31491\"%; s%:8545%:31445%; s%:8546%:31446%; s%:6065%:31465%" $HOME/.arkeo/config/app.toml

sed -i \
  -e 's|^pruning *=.*|pruning = "custom"|' \
  -e 's|^pruning-keep-recent *=.*|pruning-keep-recent = "100"|' \
  -e 's|^pruning-keep-every *=.*|pruning-keep-every = "0"|' \
  -e 's|^pruning-interval *=.*|pruning-interval = "10"|' \
  $HOME/.arkeo/config/app.toml

sed -i -e 's|^indexer *=.*|indexer = "null"|' $HOME/.arkeo/config/config.toml

sed -i 's/max_num_inbound_peers =.*/max_num_inbound_peers = 50/g' $HOME/.arkeo/config/config.toml
sed -i 's/max_num_outbound_peers =.*/max_num_outbound_peers = 50/g' $HOME/.arkeo/config/config.toml
```


## Create Service

```bash
sudo tee /etc/systemd/system/arkeod.service > /dev/null <<EOF
[Unit]
Description=Arkeo Node
After=network-online.target
[Service]
User=$USER
ExecStart=$(which arkeod) start --home $HOME/.arkeo
Restart=on-failure
RestartSec=3
LimitNOFILE=65535
[Install]
WantedBy=multi-user.target
EOF

sudo systemctl daemon-reload
sudo systemctl enable arkeo
```
## Snapshot
```bash
arkeod tendermint unsafe-reset-all --home $HOME/.arkeod --keep-addr-book
curl -L http://37.120.189.81/arkeo_testnet/arkeo_snap.tar.lz4 | tar -I lz4 -xf - -C /.arkeod
```


## Start Node And Follow Logs

```bash
sudo systemctl start arkeod && sudo journalctl -u arkeod -fo cat
```


### **Continue to** [**Become A Validator**](installation.md#become-a-validator)
