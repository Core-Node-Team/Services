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
cd /$HOME
curl -L https://github.com/CascadiaFoundation/cascadia/releases/download/v0.1.4/cascadiad -o cascadiad
chmod +x cascadiad
sudo cp cascadiad /usr/local/bin/cascadiad
rm -rf cascadiad
```

### Initalize

```bash
cascadiad config chain-id cascadia_6102-1
cascadiad config keyring-backend test
cascadiad config node tcp://localhost:11957
cascadiad init $MONIKER --chain-id cascadia_6102-1
```

### Config

```bash
curl -Ls https://raw.githubusercontent.com/0xSocrates/Testnet-Rehberler/main/Cascadia/addrbook.json > $HOME/.cascadiad/config/addrbook.json
curl -Ls https://raw.githubusercontent.com/0xSocrates/Testnet-Rehberler/main/Cascadia/genesis.json > $HOME/.cascadiad/config/genesis.json
sed -i.bak -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.0025aCC\"/" ~/.cascadiad/config/app.toml

peers="001933f36a6ec7c45b3c4cef073d0372daa5344d@194.163.155.84:49656,f78611ffa950efd9ddb4ed8f7bd8327c289ba377@65.109.108.150:46656,783a3f911d98ad2eee043721a2cf47a253f58ea1@65.108.108.52:33656,6c25f7075eddb697cb55a53a73e2f686d58b3f76@161.97.128.243:27656,8757ec250851234487f04466adacd3b1d37375f2@65.108.206.118:61556,df3cd1c84b2caa56f044ac19cf0267a44f2e87da@51.79.27.11:26656,d5519e378247dfb61dfe90652d1fe3e2b3005a5b@65.109.68.190:55656,f075e82ca89acfbbd8ef845c95bd3d50574904f5@159.69.110.238:36656,63cf1e7583eabf365856027815bc1491f2bc7939@65.108.2.41:60556,d5ba7a2288ed176ae2e73d9ae3c0edffec3caed5@65.21.134.202:16756"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.cascadiad/config/config.toml

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
curl -L http://128.140.4.67/CoreNode_Chain_Services/cascadia_snapshot.tar.lz4 | tar -I lz4 -xf - -C $HOME/.cascadiad/data
```

### Create Service

```bash
sudo tee /etc/systemd/system/cascadiad.service > /dev/null <<EOF
[Unit]
Description=Cascadia Node
After=network-online.target
[Service]
User=$USER
ExecStart=$(which cascadiad) start --home $HOME/.cascadiad
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
