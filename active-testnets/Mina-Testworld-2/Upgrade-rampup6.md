```
systemctl --user stop mina
```


```
sudo rm /etc/apt/sources.list.d/mina*.list
echo "deb [trusted=yes] http://packages.o1test.net/ focal rampup" | sudo tee /etc/apt/sources.list.d/mina-rampup.list
sudo apt-get update && sudo apt upgrade
```
```
sudo apt-get install -y mina-berkeley=2.0.0rampup6-4061884
```
```
systemctl --user restart mina
```
