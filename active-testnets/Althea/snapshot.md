# Snapshot

```bash
sudo apt install liblz4-tool

systemctl stop althea

cp $HOME/.althea/data/priv_validator_state.json $HOME/.althea/priv_validator_state.json.backup

althea tendermint unsafe-reset-all --home $HOME/.althea --keep-addr-book

curl -L http://37.120.189.81/althea_testnet/althea_snap.tar.lz4 | tar -I lz4 -xf - -C /.althea

mv $HOME/.althea/priv_validator_state.json.backup $HOME/.althea/data/priv_validator_state.json

sudo systemctl start althea && sudo journalctl -u althea -fo cat
```
### Bizi takip edin [Twitter](https://twitter.com/corenodeHQ)
### Topluluğumuza katılın [Telegram](https://t.me/corenodechat)
