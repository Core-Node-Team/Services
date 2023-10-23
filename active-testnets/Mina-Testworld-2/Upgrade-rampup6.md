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

### `mina client status` should return `Git SHA-1: 4061884b18137c1182c7fcfa80f52804008a2509`
