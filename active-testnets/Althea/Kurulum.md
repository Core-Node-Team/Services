# Kurulum

![althea](https://github.com/Core-Node-Team/Services/assets/108215275/9edeb404-fd95-4301-994e-c9242a5ffbb4)

<table data-full-width="false"><thead><tr><th align="center">Chain-ID</th><th align="center">Son Versiyon</th><th align="center">Özel Port</th></tr></thead><tbody><tr><td align="center"><mark style="color:orange;">althea_417834-3</mark></td><td align="center"><mark style="color:green;">v0.5.5</mark></td><td align="center"><mark style="color:yellow;">315</mark></td></tr></tbody></table>


> ## Sistem Gereksinimleri
<table data-header-hidden data-full-width="false"><thead><tr><th width="247">Sistem Gereksinimleri</th><th></th></tr></thead><tbody><tr><td>Minimum</td><td>4CPU 8RAM 100GB</td></tr><tr><td>Tavsiye Edilen</td><td>4CPU 16RAM 200GB</td></tr></tbody></table>

## Otomatik kurulum için komutu girin ve yönergeleri takip edin
```bash
curl -sSL -o althea.sh https://raw.githubusercontent.com/Core-Node-Team/scripts/main/althea/install.sh && chmod +x althea.sh && bash ./althea.sh && source $HOME/.bash_profile && rm althea.sh
```



## Manuel kurulum için [rehber](active-testnets/Althea/Manuel%20Kurulum.md)'i takip edin

## Validatör Olun

### Cüzdan Oluşturun

* Mnemonic'i kaydetmeyi unutmayın

```bash
althea keys add wallet
```
#### Test Tokenları Edinin

Althea [Discord](https://discord.gg/f3WC88zYYQ)'a gidin ve isteyin

#### Sync Durumunu Kontrol  Edin

* Çıktısı _<mark style="color:green;">**False**</mark>_ olmalı

```bash
althea status 2>&1 | jq .SyncInfo
```

#### Validatör Oluşturun

```bash
althea tx staking create-validator \
--amount 1000000000000000000aalthea \
--pubkey $(althea tendermint show-validator) \
--moniker "MONIKER" \
--identity "KEYBASE_ID" \
--details "Core Node Community" \
--website "WEBSITE_URL" \
--chain-id althea_417834-3 \
--commission-rate 0.1 \
--commission-max-rate 0.20 \
--commission-max-change-rate 0.03 \
--min-self-delegation 1 \
--from wallet \
--gas-adjustment 1.5 \
--gas auto \
--gas-prices 40aalthea \
-y
```

#### Validatör Detaylarını Kontrol Edin

```bash
althea q staking validator $(althea keys show wallet --bech val -a)
```

### <mark style="color:purple;">Tebrikler, Althea Ağında Bir Validatörsünüz</mark>

#### Priv Validator Key'i Yedeklemeyi Unutmayın

```bash
cat $HOME/.althea/config/priv_validator_key.json
```


