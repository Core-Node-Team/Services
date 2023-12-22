# Snapshot
```bash
sudo apt install liblz4-tool

systemctl stop pryzmd

cp $HOME/.pryzm/data/priv_validator_state.json $HOME/.pryzm/priv_validator_state.json.backup

pryzmd tendermint unsafe-reset-all --home $HOME/.pryzm --keep-addr-book

curl -L http://37.120.189.81/pryzm_testnet/pryzm_snap.tar.lz4 | tar -I lz4 -xf - -C .pryzm/data

mv $HOME/.pryzm/priv_validator_state.json.backup $HOME/.pryzm/data/priv_validator_state.json

sudo systemctl start pryzmd && sudo journalctl -u pryzmd -fo cat
```
