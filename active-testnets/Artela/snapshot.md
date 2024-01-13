# Snapshot

```bash
sudo apt install liblz4-tool

systemctl stop artelad

cp $HOME/.artelad/data/priv_validator_state.json $HOME/.artelad/priv_validator_state.json.backup

artelad tendermint unsafe-reset-all --home $HOME/.artelad --keep-addr-book

curl -L http://37.120.189.81/artela_testnet/artela_snap.tar.lz4 | tar -I lz4 -xf - -C $HOME/.artelad

mv $HOME/.artelad/priv_validator_state.json.backup $HOME/.artelad/data/priv_validator_state.json

sudo systemctl start artelad && sudo journalctl -u artelad -fo cat
```
### Bizi takip edin [Twitter](https://twitter.com/corenodeHQ)
### Topluluğumuza katılın [Telegram](https://t.me/corenodechat)
