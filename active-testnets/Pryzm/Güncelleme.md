## Pryzm update v11
```
systemctl stop pryzmd
wget https://storage.googleapis.com/pryzm-zone/core/0.11.1/pryzmd-0.11.1-linux-amd64?checksum=sha256:92d6fc71dfb49f355881915a14a8d9b5a11ac41ab89b6873191e0a56c9be4ce2
chmod +x pryzmd-0.11.1-linux-amd64?checksum=sha256:92d6fc71dfb49f355881915a14a8d9b5a11ac41ab89b6873191e0a56c9be4ce2
mv pryzmd-0.11.1-linux-amd64?checksum=sha256:92d6fc71dfb49f355881915a14a8d9b5a11ac41ab89b6873191e0a56c9be4ce2 $(which pryzmd)
systemctl restart pryzmd
```
```
journalctl -u pryzmd -fo cat
```
## Hızlı yakalayalım beklemeyelim
```
sudo apt install liblz4-tool

systemctl stop pryzmd

cp $HOME/.pryzm/data/priv_validator_state.json $HOME/.pryzm/priv_validator_state.json.backup

pryzmd tendermint unsafe-reset-all --home $HOME/.pryzm --keep-addr-book

curl -L http://37.120.189.81/pryzm_testnet/pryzm_snap.tar.lz4 | tar -I lz4 -xf - -C $HOME/.pryzm

mv $HOME/.pryzm/priv_validator_state.json.backup $HOME/.pryzm/data/priv_validator_state.json

sudo systemctl start pryzmd && sudo journalctl -u pryzmd -fo cat
```


# Güncellemeler v10

### Güncelleme Version 0.10.0 (316000)

### Güncelleme scriptini çalıştırmak için bir screen açın

```
screen -S update
```

### İndirin ve çalıştırın

```
curl -sSL -o update-0.10.0.sh https://raw.githubusercontent.com/Core-Node-Team/scripts/main/pryzm/update-0.10.0.sh && chmod +x update-0.10.0.sh && bash ./update-0.10.0.sh
```

#### Belirlenen blok yüksekliğine ulaştıktan sonra node güncellenecek

#### Screenden çıkabilirsiniz `CTRL A D` ile

#### Tamamladıktan sonra scripti silebilirsiniz `rm update-0.10.0.sh`

### Manuel güncellemek için <a href="#manuel-guencelleme" id="manuel-guencelleme"></a>

* 316000.bloğa geldiğinde

```
systemctl stop pryzmd
wget https://storage.googleapis.com/pryzm-zone/core/0.10.0/pryzmd-0.10.0-linux-amd64
chmod +x pryzmd-0.10.0-linux-amd64
mv pryzmd-0.10.0-linux-amd64 $(which pryzmd)
systemctl restart pryzmd
```
