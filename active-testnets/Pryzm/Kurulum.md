# Kurulum

![pryzm](https://github.com/Core-Node-Team/Services/assets/108215275/0d8a7f0f-0441-43f4-8e8e-32c158c9d9e3)

<table data-full-width="false"><thead><tr><th align="center">Chain-ID</th><th align="center">Son Versiyon</th><th align="center">Özel Port</th></tr></thead><tbody><tr><td align="center"><mark style="color:orange;">indigo-1</mark></td><td align="center"><mark style="color:green;">0.10.0</mark></td><td align="center"><mark style="color:yellow;">316</mark></td></tr></tbody></table>

> #### Sistem Gereksinimleri

<table data-header-hidden data-full-width="false"><thead><tr><th width="247">Sistem Gereksinimleri</th><th></th></tr></thead><tbody><tr><td>Minimum</td><td>4CPU 8RAM 200GB</td></tr><tr><td>Tavsiye Edilen</td><td>8CPU 16RAM 1TB</td></tr></tbody></table>

## Otomatik kurulum için komutu girin ve yönergeleri takip edin

```bash
curl -sSL -o pryzm.sh https://raw.githubusercontent.com/Core-Node-Team/scripts/main/pryzm/install.sh && chmod +x pryzm.sh && bash ./pryzm.sh && source $HOME/.bash_profile && rm pryzm.sh
```

### Explorer Link [BURAYA TIKLA](https://testnets.cosmosrun.info/pryzm-indigo-1/staking)

## Manuel kurulum için [rehber](active-testnets/Pryzm/Manuel%20Kurulum.md)'i takip edin

## Validatör Olun

### Cüzdan Oluşturun

* Mnemonic'i kaydetmeyi unutmayın

```bash
pryzmd keys add wallet
```

#### Test Tokenları Edinin

Prysm [Faucet](https://testnet.pryzm.zone/faucet)'a gidin

#### Sync Durumunu Kontrol Edin

* Çıktısı _<mark style="color:green;">**False**</mark>_ olmalı

```bash
pryzmd status 2>&1 | jq .SyncInfo
```

#### Validatör Oluşturun

```bash
pryzmd tx staking create-validator \
--amount 1000000upryzm \
--pubkey $(pryzmd tendermint show-validator) \
--moniker "$MONIKER" \
--identity "KEYBASE_ID" \
--details "Core Node Community" \
--website "WEBSITE_URL" \
--chain-id indigo-1 \
--commission-rate 0.1 \
--commission-max-rate 0.20 \
--commission-max-change-rate 0.03 \
--min-self-delegation 1 \
--from wallet \
--gas-adjustment 1.5 \
--gas auto \
--gas-prices 0.015upryzm \
-y
```

#### Validatör Detaylarını Kontrol Edin

```bash
pryzmd q staking validator $(pryzmd keys show wallet --bech val -a)
```

### <mark style="color:purple;">Tebrikler, Pryzm Ağında Bir Validatörsünüz</mark>

#### Priv Validator Key'i Yedeklemeyi Unutmayın

```bash
cat $HOME/.pryzm/config/priv_validator_key.json
```

### Bizi takip edin [Twitter](https://twitter.com/corenodeHQ)

### Topluluğumuza katılın [Telegram](https://t.me/corenodechat)
