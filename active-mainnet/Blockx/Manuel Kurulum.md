






### Update

sudo apt -q update
sudo apt -qy install curl git jq lz4 build-essential
sudo apt -qy upgrade

### İnstall Go

sudo rm -rf /usr/local/go
curl -Ls https://go.dev/dl/go1.20.5.linux-amd64.tar.gz | sudo tar -xzf - -C /usr/local
eval $(echo 'export PATH=$PATH:/usr/local/go/bin' | sudo tee /etc/profile.d/golang.sh)
eval $(echo 'export PATH=$PATH:$HOME/go/bin' | tee -a $HOME/.profile)

### Binaries

cd $HOME
rm -rf blockx
git clone https://github.com/BlockXLabs/BlockX-Genesis-Mainnet1 blockx
cd blockx
git checkout c940d186c0d118ea017f6abc00225fdd9b26fe14
make build



mkdir -p $HOME/.blockxd/cosmovisor/genesis/bin
mv build/blockxd $HOME/.blockxd/cosmovisor/genesis/bin/
rm -rf build

### symlinks

sudo ln -s $HOME/.blockxd/cosmovisor/genesis $HOME/.blockxd/cosmovisor/current -f
sudo ln -s $HOME/.blockxd/cosmovisor/current/bin/blockxd /usr/local/bin/blockxd -f

### Cosmovisor Setup

go install cosmossdk.io/tools/cosmovisor/cmd/cosmovisor@v1.5.0

### Service

sudo tee /etc/systemd/system/blockx.service > /dev/null << EOF
[Unit]
Description=blockx node service
After=network-online.target
 
[Service]
User=$USER
ExecStart=$(which cosmovisor) run start
Restart=on-failure
RestartSec=10
LimitNOFILE=65535
Environment="DAEMON_HOME=$HOME/.blockxd"
Environment="DAEMON_NAME=blockxd"
Environment="UNSAFE_SKIP_BACKUP=true"
 
[Install]
WantedBy=multi-user.target
EOF

### Enable Service

sudo systemctl daemon-reload
sudo systemctl enable blockx

### Initialize Node

Setting node configuration
blockxd config chain-id blockx_100-1
blockxd config keyring-backend file
blockxd config node tcp://localhost:12257

### Initialize node
blockxd init $MONIKER --chain-id blockx_100-1

### Download Genesis & Addrbook

curl -Ls http://37.120.189.81/blockx_mainnet/genesis.json > $HOME/.blockxd/config/genesis.json
curl -Ls http://37.120.189.81/blockx_mainnet/addrbook.json > $HOME/.blockxd/config/addrbook.json


### Configure Seeds

sed -i -e "s|^seeds *=.*|seeds = \"5f5cfac5c38506fbb4275c19e87c4107ec48808d@seeds.nodex.one:12210\"|" $HOME/.blockxd/config/config.toml

### Configure Gas Prices

sed -i -e "s|^minimum-gas-prices *=.*|minimum-gas-prices = \"0abcx\"|" $HOME/.blockxd/config/app.toml

### Pruning Setting

sed -i \
  -e 's|^pruning *=.*|pruning = "custom"|' \
  -e 's|^pruning-keep-recent *=.*|pruning-keep-recent = "100"|' \
  -e 's|^pruning-keep-every *=.*|pruning-keep-every = "0"|' \
  -e 's|^pruning-interval *=.*|pruning-interval = "19"|' \
  $HOME/.blockxd/config/app.toml

### Custom Port

sed -i -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:12258\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:12257\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:12260\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:12256\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":12266\"%" $HOME/.blockxd/config/config.toml
sed -i -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:12217\"%; s%^address = \":8080\"%address = \":12280\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:12290\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:12291\"%; s%:8545%:12245%; s%:8546%:12246%; s%:6065%:12265%" $HOME/.blockxd/config/app.toml

### Download Snapshots

curl -L http://37.120.189.81/blockx_mainnet/blockx_snap.tar.lz4 | tar -Ilz4 -xf - -C $HOME/.blockxd
[[ -f $HOME/.blockxd/data/upgrade-info.json ]] && cp $HOME/.blockxd/data/upgrade-info.json $HOME/.blockxd/cosmovisor/genesis/upgrade-info.json

### Start Service

systemctl daemon-reload && sudo systemctl start blockx && journalctl -u blockxd -fo cat



