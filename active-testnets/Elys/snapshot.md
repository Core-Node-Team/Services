# Snapshot

```
sudo apt install liblz4-tool

sudo systemctl stop elysd

cp $HOME/.elys/data/priv_validator_state.json $HOME/.elys/priv_validator_state.json.backup

elysd tendermint unsafe-reset-all

curl -L http://202.61.243.24/CoreNode_ChainServices/elys_snapshot.tar.lz4 | tar -I lz4 -xf - -C $HOME/.elys/data

mv $HOME/.elys/priv_validator_state.json.backup $HOME/.elys/data/priv_validator_state.json

sudo systemctl start elysd && sudo journalctl -u elysd -fo cat
```
