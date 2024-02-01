### Update ve gereklilikleri yükleyelim

```
sudo apt update
sudo apt install make clang pkg-config libssl-dev build-essential
```

### Dosya olusturuyoruz
```
cd
sudo mkdir /root/avail-node/
```
```
cd /root/avail-node/
```

### Dosyaları çekiyoruz

```
wget https://github.com/availproject/avail/releases/download/v1.10.0.0/x86_64-ubuntu-2004-data-avail.tar.gz
```

### Zipten dosyaları çı çı çıkraaa :D (çıkaralım)

```
tar xvzf x86_64-ubuntu-2004-data-avail.tar.gz
```
------------------------
### Servis oluşturalım.
NOT: molla202 yazan kısmı değiştiriniz explorerdeki görünen isminiz...
```
sudo tee /etc/systemd/system/availd.service > /dev/null <<'EOF'
[Unit]
Description=Avail Validator
After=network.target
StartLimitIntervalSec=0

[Service]
User=root
Type=simple
Restart=always
RestartSec=120
ExecStart=/root/avail-node/data-avail -d /root/avail-node/data --chain goldberg --port 30333 --validator --name "molla202" \
--reserved-nodes \
"/dns/bootnode-001.goldberg.avail.tools/tcp/30333/p2p/12D3KooWCVqFvrP3UJ1S338Gb8SHvEQ1xpENLb45Dbynk4hu1XGN" \
"/dns/bootnode-002.goldberg.avail.tools/tcp/30333/p2p/12D3KooWD6sWeWCG5Z1qhejhkPk9Rob5h75wYmPB6MUoPo7br58m" \
"/dns/bootnode-003.goldberg.avail.tools/tcp/30333/p2p/12D3KooWMR9ZoAVWJv6ahraVzUCfacNbFKk7ABoWxVL3fJ3XXGDw" \
"/dns/bootnode-004.goldberg.avail.tools/tcp/30333/p2p/12D3KooWMuyLE3aPQ82HTWuPUCjiP764ebQrZvGUzxrYGuXWZJZV" \
"/dns/bootnode-005.goldberg.avail.tools/tcp/30333/p2p/12D3KooWKJwbdcZ7QWcPLHy3EJ1UiffaLGnNBMffeK8AqRVWBZA1" \
"/dns/bootnode-006.goldberg.avail.tools/tcp/30333/p2p/12D3KooWM8AaHDH8SJvg6bq4CGQyHvW2LH7DCHbdv633dsrti7i5" \
--reserved-only

[Install]
WantedBy=multi-user.target
EOF
```

### Nodu başlatalım
```
sudo systemctl daemon-reload
sudo systemctl enable availd.service
sudo systemctl restart availd.service
```

### Durmuna bakalım
```
sudo systemctl status availd.service
```

### Log kontrol
```
journalctl -u availd.service -fo cat
```
