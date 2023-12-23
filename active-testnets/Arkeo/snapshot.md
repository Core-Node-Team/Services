# Snapshot

```bash
sudo apt install liblz4-tool

systemctl stop arkeod

cp $HOME/.arkeod/data/priv_validator_state.json $HOME/.arkeod/priv_validator_state.json.backup

arkeod tendermint unsafe-reset-all --home $HOME/.arkeod --keep-addr-book

curl -L http://37.120.189.81/arkeo_testnet/arkeo_snap.tar.lz4 | tar -I lz4 -xf - -C /.arkeod

mv $HOME/.arkeod/priv_validator_state.json.backup $HOME/.arkeod/data/priv_validator_state.json

sudo systemctl start arkeod && sudo journalctl -u arkeod -fo cat
```
