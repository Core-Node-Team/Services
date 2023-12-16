# Manuel Installation

![cascadia](https://github.com/Core-Node-Team/Gitbook/assets/108215275/da94933d-4d38-430f-bd91-7d06f68e5e72)

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
cd $HOME || return
rm -rf $HOME/cascadia
git clone https://github.com/cascadiafoundation/cascadia.git
cd $HOME/cascadia || return
git checkout v0.1.9
make install
```

### Initalize

```bash
cascadiad config chain-id cascadia_11029-1
cascadiad config keyring-backend test
cascadiad config node tcp://localhost:11957
cascadiad init $MONIKER --chain-id cascadia_11029-1
```

### Config

```bash
curl -s https://raw.githubusercontent.com/molla202/Cascadia-11029-devnet/main/genesis.json > $HOME/.cascadiad/config/genesis.json
curl -s https://raw.githubusercontent.com/molla202/Cascadia-11029-devnet/main/addrbook.json > $HOME/.cascadiad/config/addrbook.json
sed -i.bak -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.0025aCC\"/" ~/.cascadiad/config/app.toml

SEEDS=""
PEERS="21ca2712116138429aed3d72422379397c53fa86@65.109.65.248:34656"
sed -i 's|^seeds *=.*|seeds = "'$SEEDS'"|; s|^persistent_peers *=.*|persistent_peers = "'$PEERS'"|' $HOME/.cascadiad/config/config.toml

sed -i -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:11958\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:11957\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:11960\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:11956\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":11966\"%" $HOME/.cascadiad/config/config.toml
sed -i -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:11917\"%; s%^address = \":8080\"%address = \":11980\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:11990\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:11991\"%; s%:8545%:11945%; s%:8546%:11946%; s%:6065%:11965%" $HOME/.cascadiad/config/app.toml

sed -i 's/max_num_inbound_peers =.*/max_num_inbound_peers = 50/g' $HOME/.cascadiad/config/config.toml
sed -i 's/max_num_outbound_peers =.*/max_num_outbound_peers = 50/g' $HOME/.cascadiad/config/config.toml

sed -i \
  -e 's|^pruning *=.*|pruning = "custom"|' \
  -e 's|^pruning-keep-recent *=.*|pruning-keep-recent = "100"|' \
  -e 's|^pruning-keep-every *=.*|pruning-keep-every = "0"|' \
  -e 's|^pruning-interval *=.*|pruning-interval = "10"|' \
  $HOME/.cascadiad/config/app.toml

sed -i -e 's|^indexer *=.*|indexer = "null"|' $HOME/.cascadiad/config/config.toml
```

### Download Snapshot

```bash
sudo apt install liblz4-tool -y

curl -L http://snapshot.corenode.info/cascadia_testnet/cascadia_snap.tar.lz4 | tar -I lz4 -xf - -C /.cascadiad/data
```

### Create Service

```bash
sudo tee /etc/systemd/system/cascadiad.service > /dev/null <<EOF
[Unit]
Description=Cascadia Node
After=network-online.target
[Service]
User=$USER
ExecStart=$(which cascadiad) start --chain-id cascadia_11029-1 --home $HOME/.cascadiad
Restart=on-failure
RestartSec=3
LimitNOFILE=65535
[Install]
WantedBy=multi-user.target
EOF

sudo systemctl daemon-reload
sudo systemctl enable cascadiad
```

### Start Node And Foolow Logs

```bash
sudo systemctl start cascadiad && sudo journalctl -u cascadiad -fo cat
```

### **Continue To** [**Become A Validator**](installation.md#become-a-validator)
