# Manuel Installation



![babylon](https://github.com/Core-Node-Team/Gitbook/assets/108215275/fe72b6b0-bd7b-4c56-b94c-57c7c7e21eac)

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
cd /$HOME
git clone https://github.com/babylonchain/babylon.git
cd babylon
git checkout v0.7.2
make install
```

## Initalize

```bash
babylond config chain-id bbn-test-2
babylond config keyring-backend test
babylond config node tcp://localhost:31157
babylond init $MONIKER --chain-id bbn-test-2
```

## Config

```bash
curl -Ls https://raw.githubusercontent.com/Core-Node-Team/Testnet-TR/main/Babylon/addrbook.json > $HOME/.babylond/config/addrbook.json

curl -Ls https://raw.githubusercontent.com/Core-Node-Team/Testnet-TR/main/Babylon/genesis.json > $HOME/.babylond/config/genesis.json

sed -i -e "s|^minimum-gas-prices *=.*|minimum-gas-prices = \"0.00001ubbn\"|" $HOME/.babylond/config/app.toml

peers="603f5e1d2b796e125265b975135e1780e080e8fd@138.201.204.5:37656,07d1b69e4dc56d46dabe8f5eb277fcde0c6c9d1e@23.88.5.169:17656,0a0d7d245ea67cfeec7d000085260fbe695544f4@207.180.251.220:11656,924632d809935a2ba6035df0dd3a787dde7b788c@144.76.201.43:26356,1016bb6d890ffafe49eb8b2264937bdbcd775135@46.4.5.45:20656,4c4a3cffbbad5e12c2f4d1ee85f6a94eb271ae21@65.109.90.171:32656,d5519e378247dfb61dfe90652d1fe3e2b3005a5b@65.109.68.190:16456,37896797924b1cb18bce9b9542ba3da915a85038@18.218.71.198:26656,ec92965f98006978a470642a9e02f971fb4a70c6@18.119.146.153:26656,0de44b3d4380004838d38797a1aee10392b68420@3.18.176.128:26656,"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.babylond/config/config.toml

sed -i -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:31158\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:31157\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:31160\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:31156\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":31166\"%" $HOME/.babylond/config/config.toml
sed -i -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:31117\"%; s%^address = \":8080\"%address = \":31180\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:31190\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:31191\"%; s%:8545%:31145%; s%:8546%:31146%; s%:6065%:31165%" $HOME/.babylond/config/app.toml

sed -i 's/max_num_inbound_peers =.*/max_num_inbound_peers = 50/g' $HOME/.babylond/config/config.toml
sed -i 's/max_num_outbound_peers =.*/max_num_outbound_peers = 50/g' $HOME/.babylond/config/config.toml

sed -i \
  -e 's|^pruning *=.*|pruning = "custom"|' \
  -e 's|^pruning-keep-recent *=.*|pruning-keep-recent = "100"|' \
  -e 's|^pruning-keep-every *=.*|pruning-keep-every = "0"|' \
  -e 's|^pruning-interval *=.*|pruning-interval = "10"|' \
  $HOME/.babylond/config/app.toml

sed -i -e 's|^indexer *=.*|indexer = "null"|' $HOME/.babylond/config/config.toml
```

## Download Snapshot

```bash
sudo apt install liblz4-tool -y

curl -L http://snapshot.corenode.info/babylon_testnet/babylon_snap.tar.lz4 | tar -I lz4 -xf - -C /.babylond/data
```

## Create Service

```bash
sudo tee /etc/systemd/system/babylond.service > /dev/null <<EOF
[Unit]
Description=Babylon Node
After=network-online.target
[Service]
User=$USER
ExecStart=$(which babylond) start --home $HOME/.babylond
Restart=on-failure
RestartSec=3
LimitNOFILE=65535
[Install]
WantedBy=multi-user.target
EOF

sudo systemctl daemon-reload
sudo systemctl enable babylond
```

## Start Node And Follow Logs

```bash
sudo systemctl start babylond && sudo journalctl -u babylond -fo cat
```

### **Continue to** [**Become A Validator**](installation.md#become-a-validator)
