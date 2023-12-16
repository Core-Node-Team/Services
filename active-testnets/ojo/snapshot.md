# Snapshot

```bash
sudo apt install liblz4-tool

systemctl stop ojod

cp $HOME/.ojo/data/priv_validator_state.json $HOME/.ojo/priv_validator_state.json.backup

ojod tendermint unsafe-reset-all --home $HOME/.ojo --keep-addr-book 

curl -L http://snapshot.corenode.info/ojo_testnet/ojo_snap.tar.lz4 | tar -I lz4 -xf - -C $HOME/.ojo/data

mv $HOME/.ojo/priv_validator_state.json.backup $HOME/.ojo/data/priv_validator_state.json 

sudo systemctl start ojod && sudo journalctl -u ojod -fo cat
```
