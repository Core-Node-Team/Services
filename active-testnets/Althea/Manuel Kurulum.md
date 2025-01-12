# Manuel Kurulum

![logo-blue-orange-dot](https://github.com/molla202/Babylon/assets/91562185/fdae1a09-6805-4384-9a58-62c279ed89e1)

## Althea Network L1

* [Topluluk kanalımız](https://t.me/corenodechat)\

* [Topluluk Twitter](https://twitter.com/corenodeHQ)\

* [Core Node Site](https://corenode.info/)\

* [AltheaNetwork Website](https://www.althea.net/)\

* [Blockchain Explorer](https://althea.explorers.guru/)\

* [Discord](https://discord.gg/V7NGnHq3)\

* [AltheaNetwork Twitter](https://twitter.com/AltheaNetwork)\


### Sistem Gereksinimleri

| Bileşenler                                                | Minimum Gereksinimler |
| --------------------------------------------------------- | --------------------- |
| ✔️CPU                                                     | 4                     |
| ✔️RAM                                                     | 8 GB                  |
| ✔️Storage                                                 | 250 GB SSD            |
| ✔️UBUNTU                                                  | 22                    |
| ### Update ve kütüphane kurulumu                          |                       |
| \`\`\`                                                    |                       |
| sudo apt update                                           |                       |
| sudo apt install -y curl git jq lz4 build-essential unzip |                       |

```
### Go kurulumu yapalım
```

cd $HOME ! \[ -x "$(command -v go)" ] && { VER="1.19.3" wget "https://golang.org/dl/go$VER.linux-amd64.tar.gz" sudo rm -rf /usr/local/go sudo tar -C /usr/local -xzf "go$VER.linux-amd64.tar.gz" rm "go$VER.linux-amd64.tar.gz" \[ ! -f \~/.bash\_profile ] && touch ~~/.bash\_profile echo "export PATH=$PATH:/usr/local/go/bin:~~/go/bin" >> \~/.bash\_profile source $HOME/.bash\_profile } \[ ! -d \~/go/bin ] && mkdir -p \~/go/bin

```
```

## Download project binaries

mkdir -p $HOME/.althea/cosmovisor/genesis/bin wget -O $HOME/.althea/cosmovisor/genesis/bin/althea https://github.com/althea-net/althea-L1/releases/download/v1.0.0-rc1/althea-linux-amd64 chmod +x $HOME/.althea/cosmovisor/genesis/bin/althea

```
```

## Create application symlinks

sudo ln -s $HOME/.althea/cosmovisor/genesis $HOME/.althea/cosmovisor/current -f sudo ln -s $HOME/.althea/cosmovisor/current/bin/althea /usr/local/bin/althea -f

```
```

## Download and install Cosmovisor

go install cosmossdk.io/tools/cosmovisor/cmd/cosmovisor@v1.4.0

```
```

## Create service

sudo tee /etc/systemd/system/althea.service > /dev/null << EOF \[Unit] Description=althea node service After=network-online.target

\[Service] User=$USER ExecStart=$(which cosmovisor) run start Restart=on-failure RestartSec=10 LimitNOFILE=65535 Environment="DAEMON\_HOME=$HOME/.althea" Environment="DAEMON\_NAME=althea" Environment="UNSAFE\_SKIP\_BACKUP=true" Environment="PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:$HOME/.althea/cosmovisor/current/bin"

\[Install] WantedBy=multi-user.target EOF

```
```

sudo systemctl daemon-reload sudo systemctl enable althea

```
```

## Set node configuration

althea config chain-id althea\_417834-4 althea config keyring-backend test althea config node tcp://localhost:15257

```
### $MONIKER değiştirelim.
```

## Initialize the node

althea init $MONIKER --chain-id althea\_417834-4

```
```

## Download genesis and addrbook

curl -Ls https://github.com/molla202/ALTHEA/raw/main/genesis.json > $HOME/.althea/config/genesis.json curl -Ls https://github.com/molla202/ALTHEA/raw/main/addrbook.json > $HOME/.althea/config/addrbook.json

```
```

sed -i.bak -e "s/^minimum-gas-prices _=._/minimum-gas-prices = "0.0aalthea"/;" \~/.althea/config/app.toml external\_address=$(wget -qO- eth0.me) sed -i.bak -e "s/^external\_address _=._/external\_address = "$external\_address:26656"/" $HOME/.althea/config/config.toml peers="ab9a9e6ea747839652dfe4480e66a5eb78a385e8@51.81.167.60:17200,cbdcc6edc9b2cbd652fe94ef774e1f483095a8a3@66.172.36.142:14656" sed -i.bak -e "s/^persistent\_peers _=._/persistent\_peers = "$peers"/" $HOME/.althea/config/config.toml seeds="" sed -i.bak -e "s/^seeds =._/seeds = "$seeds"/" $HOME/.althea/config/config.toml sed -i 's/max\_num\_inbound\_peers =._/max\_num\_inbound\_peers = 50/g' $HOME/.althea/config/config.toml sed -i 's/max\_num\_outbound\_peers =.\*/max\_num\_outbound\_peers = 50/g' $HOME/.althea/config/config.toml

indexer="null" &&\
sed -i -e "s/^indexer _=._/indexer = "$indexer"/" $HOME/.althea/config/config.toml

## Set pruning

pruning="custom" pruning\_keep\_recent="1000" pruning\_keep\_every="0" pruning\_interval="10" sed -i -e "s/^pruning _=._/pruning = "$pruning"/" $HOME/.althea/config/app.toml sed -i -e "s/^pruning-keep-recent _=._/pruning-keep-recent = "$pruning\_keep\_recent"/" $HOME/.althea/config/app.toml sed -i -e "s/^pruning-keep-every _=._/pruning-keep-every = "$pruning\_keep\_every"/" $HOME/.althea/config/app.toml sed -i -e "s/^pruning-interval _=._/pruning-interval = "$pruning\_interval"/" $HOME/.althea/config/app.toml

```
### Port farklı olsun derseniz. PORT:15
```

## Set custom ports

sed -i -e "s%^proxy\_app = "tcp://127.0.0.1:26658"%proxy\_app = "tcp://127.0.0.1:15258"%; s%^laddr = "tcp://127.0.0.1:26657"%laddr = "tcp://127.0.0.1:15257"%; s%^pprof\_laddr = "localhost:6060"%pprof\_laddr = "localhost:15260"%; s%^laddr = "tcp://0.0.0.0:26656"%laddr = "tcp://0.0.0.0:15256"%; s%^prometheus\_listen\_addr = ":26660"%prometheus\_listen\_addr = ":15266"%" $HOME/.althea/config/config.toml sed -i -e "s%^address = "tcp://0.0.0.0:1317"%address = "tcp://0.0.0.0:15217"%; s%^address = ":8080"%address = ":15280"%; s%^address = "0.0.0.0:9090"%address = "0.0.0.0:15290"%; s%^address = "0.0.0.0:9091"%address = "0.0.0.0:15291"%; s%:8545%:15245%; s%:8546%:15246%; s%:6065%:15265%" $HOME/.althea/config/app.toml

```
### Snap Atam hızlı olsun
```

cd $HOME apt install lz4 sudo systemctl stop althea cp $HOME/.althea/data/priv\_validator\_state.json $HOME/.althea/priv\_validator\_state.json.backup rm -rf $HOME/.althea/data curl -o - -L http://37.120.189.81/althea\_testnet/althea\_snap.tar.lz4 | lz4 -c -d - | tar -x -C $HOME/.althea --strip-components 2 mv $HOME/.althea/priv\_validator\_state.json.backup $HOME/.althea/data/priv\_validator\_state.json

```
### Hadi başlatalım
```

sudo systemctl restart althea && sudo journalctl -u althea -f --no-hostname -o cat

```

### Cüzdan Oluşturma

* Yeni Cüzdan
```

althea keys add wallet

```
* Cüzdan import
```

althea keys add wallet --recover

```
### Validator kurulumu

NOT: Moniker adınızı ve cüzdan adınızı yazınız.
```

althea tx staking create-validator\
\--amount 1000000aalthea\
\--pubkey $(althea tendermint show-validator)\
\--moniker "YOUR\_MONIKER\_NAME"\
\--identity "YOUR\_KEYBASE\_ID"\
\--details "YOUR\_DETAILS"\
\--website "YOUR\_WEBSITE\_URL"\
\--chain-id althea\_417834-4\
\--commission-rate 0.05\
\--commission-max-rate 0.20\
\--commission-max-change-rate 0.01\
\--min-self-delegation 1\
\--from wallet\
\--gas-adjustment 1.4\
\--gas auto\
\--gas-prices 0aalthea\
-y

```
```
