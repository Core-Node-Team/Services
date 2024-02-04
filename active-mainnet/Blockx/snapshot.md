# Snapshot

```bash
sudo apt install liblz4-tool

systemctl stop blockxd

cp $HOME/.blockxd/data/priv_validator_state.json $HOME/.blockxd/priv_validator_state.json.backup

babylond tendermint unsafe-reset-all --home $HOME/.blockxd --keep-addr-book

curl -L http://37.120.189.81/blockx_mainnet/blockx_snap.tar.lz4 | tar -I lz4 -xf - -C $HOME/.blockxd

mv $HOME/.blockxd/priv_validator_state.json.backup $HOME/.blockxd/data/priv_validator_state.json

sudo systemctl start blockxd && sudo journalctl -u blockxd -fo cat
```
### Bizi takip edin [Twitter](https://twitter.com/corenodeHQ)
### Topluluğumuza katılın [Telegram](https://t.me/corenodechat)
