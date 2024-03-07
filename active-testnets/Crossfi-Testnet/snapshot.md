# Snapshot

```bash
sudo apt install liblz4-tool

systemctl stop crossfid

cp $HOME/.mineplex-chain/data/priv_validator_state.json $HOME/.mineplex-chain/priv_validator_state.json.backup

crossfid tendermint unsafe-reset-all --home $HOME/.mineplex-chain --keep-addr-book

curl -L http://37.120.189.81/crossfi_testnet/crossfi_snap.tar.lz4 | tar -I lz4 -xf - -C $HOME/.mineplex-chain

mv $HOME/.mineplex-chain/priv_validator_state.json.backup $HOME/.mineplex-chain/data/priv_validator_state.json

sudo systemctl restart crossfid && sudo journalctl -u crossfid -fo cat
```
### Bizi takip edin [Twitter](https://twitter.com/corenodeHQ)
### Topluluğumuza katılın [Telegram](https://t.me/corenodechat)
