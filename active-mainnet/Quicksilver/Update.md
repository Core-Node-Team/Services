


![image](https://github.com/Core-Node-Team/Testnet-TR/assets/91562185/0d7cf06e-61ae-41ba-a430-77170de2b1e4)


* [Twitter](https://twitter.com/quicksilverzone)
* [Telegram](https://t.me/quicksilverzone)
* [Discord](https://discord.gg/DBg2Vr3x)
* [Website](https://quicksilver.zone/)
* [Docs](https://docs.quicksilver.zone/)
* [Github](https://github.com/ingenuity-build/)
* [Quicksilver Liquid Staking Protocol](https://app.quicksilver.zone/)

>## v1.5.1 has been released, as is slated for Monday 18th March at around 15:00 UTC (block 6452000).

## Manuel
```
cd $HOME
wget -O quicksilverd https://github.com/ingenuity-build/quicksilver/releases/download/v1.5.1/quicksilverd-v1.5.1-amd64
mv quicksilverd-v1.5.1-amd64 quicksilverd
chmod +x quicksilverd
sudo mv $HOME/quicksilverd $(which quicksilverd)
sudo systemctl restart quicksilverd && sudo journalctl -u quicksilverd -f
```
OR
```
cd $HOME
rm -rf quicksilver
git clone https://github.com/ingenuity-build/quicksilver.git
cd quicksilver
git checkout v1.5.1
make install
```
## Cosmovisor
```
cd $HOME
rm -rf quicksilver
git clone https://github.com/ingenuity-build/quicksilver.git
cd quicksilver
git checkout v1.5.1
```
### Build binaries
```
make build
```
### Prepare binaries for Cosmovisor
```
mkdir -p $HOME/.quicksilverd/cosmovisor/upgrades/v1.5.1/bin
mv build/quicksilverd $HOME/.quicksilverd/cosmovisor/upgrades/v1.5.1/bin/
rm -rf build
```
