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
wget https://github.com/availproject/avail/releases/download/v1.8.0.3/amd64-ubuntu-2204-data-avail.tar.gz
```
### Zipten dosyaları çı çı çıkraaa :D (çıkaralım)
```
tar xvzf amd64-ubuntu-2204-data-avail.tar.gz
```
```
mv amd64-ubuntu-2204-data-avail data-avail
```
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
ExecStart=/root/avail-node/data-avail -d /root/avail-node/data --chain goldberg --port 30333 --validator --name "molla202"

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
journalctl -f -u availd.service
```
