# Manuel Installation



![elys](https://github.com/Core-Node-Team/Gitbook/assets/108215275/41c4a299-4ad1-4ac4-bcc3-367904fb10fb)

### Prepare Server

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

### Install Binary

```bash
ROCKSDB_VERSION=8.9.1
sudo apt install -y libgflags-dev libsnappy-dev zlib1g-dev libbz2-dev liblz4-dev libzstd-dev
cd /tmp
wget https://github.com/facebook/rocksdb/archive/refs/tags/v${ROCKSDB_VERSION}.tar.gz
tar -xvf v${ROCKSDB_VERSION}.tar.gz && cd rocksdb-${ROCKSDB_VERSION} || return
export CXXFLAGS='-Wno-error=deprecated-copy -Wno-error=pessimizing-move -Wno-error=class-memaccess'
make shared_lib
sudo make install-shared INSTALL_PATH=/usr
rm -rf /tmp/rocksdb-${ROCKSDB_VERSION} /tmp/v${ROCKSDB_VERSION}.tar.gz
export LD_LIBRARY_PATH=/usr/local/lib
cd $HOME
rm -rf elys
git clone https://github.com/elys-network/elys.git
cd elys
git checkout build/rocksdb
git tag v0.29.26 -d
git tag v0.29.26
make install
```

### Initalize

```bash
elysd config chain-id elystestnet-1
elysd config keyring-backend test
elysd config node tcp://localhost:31357
elysd init <MONIKER> --chain-id elystestnet-1
```

### Config

```bash
curl -Ls https://raw.githubusercontent.com/Core-Node-Team/Testnet-TR/main/Elys/addrbook.json > $HOME/.elys/config/addrbook.json
curl -Ls https://raw.githubusercontent.com/Core-Node-Team/Testnet-TR/main/Elys/genesis.json > $HOME/.elys/config/genesis.json
peers="258f523c96efde50d5fe0a9faeea8a3e83be22ca@seed.elystestnet-1.elys.aviaone.com:20273"
seeds="ae7191b2b922c6a59456588c3a262df518b0d130@elys-testnet-seed.itrocket.net:38656"
sed -i -e 's|^seeds *=.*|seeds = "'$seeds'"|; s|^persistent_peers *=.*|persistent_peers = "'$peers'"|' $HOME/.elys/config/config.toml
sed -i 's/minimum-gas-prices =.*/minimum-gas-prices = "0.0uelys"/g' $HOME/.elys/config/app.toml

sed -i 's/max_num_inbound_peers =.*/max_num_inbound_peers = 50/g' $HOME/.elys/config/config.toml
sed -i 's/max_num_outbound_peers =.*/max_num_outbound_peers = 50/g' $HOME/.elys/config/config.toml

sed -i \
  -e 's|^pruning *=.*|pruning = "custom"|' \
  -e 's|^pruning-keep-recent *=.*|pruning-keep-recent = "100"|' \
  -e 's|^pruning-keep-every *=.*|pruning-keep-every = "0"|' \
  -e 's|^pruning-interval *=.*|pruning-interval = "10"|' \
  $HOME/.elys/config/app.toml

sed -i -e 's|^indexer *=.*|indexer = "null"|' $HOME/.elys/config/config.toml

CustomPort="313"
sed -i -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:${CustomPort}58\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:${CustomPort}57\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:${CustomPort}60\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:${CustomPort}56\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":${CustomPort}66\"%" $HOME/$DirectName/config/config.toml
sed -i -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:${CustomPort}17\"%; s%^address = \":8080\"%address = \":${CustomPort}80\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:${CustomPort}90\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:${CustomPort}91\"%; s%:8545%:${CustomPort}45%; s%:8546%:${CustomPort}46%; s%:6065%:${CustomPort}65%" $HOME/$DirectName/config/app.toml
sed -i -e "s%^address = \"tcp://localhost:1317\"%address = \"tcp://localhost:${CustomPort}17\"%; s%^address = \":8080\"%address = \":${CustomPort}80\"%; s%^address = \"localhost:9090\"%address = \"localhost:${CustomPort}90\"%; s%^address = \"localhost:9091\"%address = \"localhost:${CustomPort}91\"%; s%:8545%:${CustomPort}45%; s%:8546%:${CustomPort}46%; s%:6065%:${CustomPort}65%" $HOME/$DirectName/config/app.toml
```

### Download Snapshot

```bash
sudo apt install liblz4-tool -y

curl -L http://37.120.189.81/elys_testnet/elys_snap.tar.lz4 | tar -I lz4 -xf - -C $HOME/.elysd
```

### Create Service

```bash
sudo tee /etc/systemd/system/elysd.service > /dev/null <<EOF
[Unit]
Description=Elys node
After=network-online.target
[Service]
User=$USER
WorkingDirectory=$HOME/.elys
ExecStart=$(which elysd) start --minimum-gas-prices="0.0018ibc/2180E84E20F5679FCC760D8C165B60F42065DEF7F46A72B447CFF1B7DC6C0A65,0.00025ibc/E2D2F6ADCC68AA3384B2F5DFACCA437923D137C14E86FB8A10207CF3BED0C8D4,0.00025uelys" --home $HOME/.elys
Restart=on-failure
RestartSec=5
LimitNOFILE=65535
Environment="LD_LIBRARY_PATH=/usr/local/lib"
[Install]
WantedBy=multi-user.target
EOF
```
```
sudo systemctl daemon-reload
sudo systemctl enable elysd
```

### Start Node And Follow Logs

```bash
sudo systemctl start elysd && sudo journalctl -u elysd -fo cat
```

### Continue To [Become A Validator](installation.md#become-a-validator)
