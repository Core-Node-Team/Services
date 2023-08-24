![image](https://github.com/Core-Node-Team/Gitbook/assets/108215275/b4e31f1c-5d7f-4540-b8dc-21988e9272ef)

## Prepare Server

```
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

```
git clone https://github.com/ojo-network/ojo
cd ojo
git checkout v0.1.2
make install
```

## Initalize And Configuration
```
ojod config chain-id ojo-devnet
ojod config keyring-backend test
ojod config node tcp://localhost:31257
ojod init <MONIKER> --chain-id ojo-devnet

sed -i 's/max_num_inbound_peers =.*/max_num_inbound_peers = 50/g' $HOME/.ojo/config/config.toml
sed -i 's/max_num_outbound_peers =.*/max_num_outbound_peers = 50/g' $HOME/.ojo/config/config.toml

sed -i \
  -e 's|^pruning *=.*|pruning = "custom"|' \
  -e 's|^pruning-keep-recent *=.*|pruning-keep-recent = "100"|' \
  -e 's|^pruning-keep-every *=.*|pruning-keep-every = "0"|' \
  -e 's|^pruning-interval *=.*|pruning-interval = "10"|' \
  $HOME/.ojo/config/app.toml

sed -i -e 's|^indexer *=.*|indexer = "null"|' $HOME/.ojo/config/config.toml

CustomPort="312"
sed -i -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:${CustomPort}58\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:${CustomPort}57\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:${CustomPort}60\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:${CustomPort}56\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":${CustomPort}66\"%" $HOME/.ojo/config/config.toml
sed -i -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:${CustomPort}17\"%; s%^address = \":8080\"%address = \":${CustomPort}80\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:${CustomPort}90\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:${CustomPort}91\"%; s%:8545%:${CustomPort}45%; s%:8546%:${CustomPort}46%; s%:6065%:${CustomPort}65%" $HOME/.ojo/config/app.toml
sed -i -e "s%^address = \"tcp://localhost:1317\"%address = \"tcp://localhost:${CustomPort}17\"%; s%^address = \":8080\"%address = \":${CustomPort}80\"%; s%^address = \"localhost:9090\"%address = \"localhost:${CustomPort}90\"%; s%^address = \"localhost:9091\"%address = \"localhost:${CustomPort}91\"%; s%:8545%:${CustomPort}45%; s%:8546%:${CustomPort}46%; s%:6065%:${CustomPort}65%" $HOME/.ojo/config/app.toml

curl -Ls https://raw.githubusercontent.com/0xSocrates/Testnet-Rehberler/main/Ojo/addrbook.json > $HOME/.ojo/config/addrbook.json
curl -Ls https://rpc.devnet-n0.ojo-devnet.node.ojo.network/genesis > $HOME/.ojo/config/genesis.json

sed -i 's|^minimum-gas-prices *=.*|minimum-gas-prices = "0.0001uojo"|g' $HOME/.ojo/config/app.toml
sed -i 's|^prometheus *=.*|prometheus = true|' $HOME/.ojo/config/config.toml
```
## Create Service
```
sudo tee /etc/systemd/system/ojod.service > /dev/null <<EOF
[Unit]
Description=Ojo Node
After=network-online.target
[Service]
User=$USER
ExecStart=$(which ojod) start --home $HOME/.ojo
Restart=on-failure
RestartSec=3
LimitNOFILE=65535
[Install]
WantedBy=multi-user.target
EOF

systemctl daemon-reload
systemctl enable ojod
```

## Start Node And Follow Logs
```
sudo systemctl start ojod && sudo journalctl -u ojod -fo cat
```
































