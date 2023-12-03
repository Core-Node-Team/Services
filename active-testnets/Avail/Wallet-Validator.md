### Cüzdan olusturma

https://goldberg.avail.tools/#/staking

- Yukarıdan account kısmına gelip bir polkadot cüzdanı bağlayın yada olusturun. varsa zaten girerken bağlanmak için sorar yoksa

https://goldberg.avail.tools/#/accounts

![image](https://github.com/Core-Node-Team/Testnet-TR/assets/91562185/24990a85-1e6b-465b-9c93-b3213e82b62c)


not: telemetry den ismizi yazarak bakınız.

https://telemetry.avail.tools/#list/0x6f09966420b2608d1947ccfb0f2a362450d1fc7fd902c29b67c906eaa965a7ae

![image](https://github.com/molla202/Avail/assets/91562185/85b2a9c3-7821-41f0-86c6-9b4a1b175cc3)



### Sezon keyi oluşturuyoruz sunucumuzda 
```
curl -H "Content-Type: application/json" -d '{"id":1, "jsonrpc":"2.0", "method": "author_rotateKeys", "params":[]}' http://localhost:9944
```
OR PORT and change up link end port number. portlara bakınız eğer farklı ise veya değiştirdiyseniz yukarıdaki linkin sonundaki portu ona göre ayarlayınız...
```
sudo lsof -i -P -n | grep LISTEN
```
![image](https://github.com/Core-Node-Team/Testnet-TR/assets/91562185/5a8cd641-6a07-4fcc-9a54-6d27a4ba49ae)

-------------------------------------------------------------------
Not:seçilirseniz validator nasıl olursunuz...

https://goldberg.avail.tools/#/staking/actions

- Sağ kısımdan validatore tıklıyoruz

![image](https://github.com/Core-Node-Team/Testnet-TR/assets/91562185/d1a6161f-6c49-46a7-82f4-2f290de3e6ea)

- Sezon keyi olusturmustuk bundaki `"result":` dan sonraki uzun adresi kopyalıyoruz
- validator demiştik açılan ekranda stake miktarı belirliyoruz ileri diyoruz
- ve bizden key soruyor yazıyoruz ileri deyip cüzdandan onaylıyoruz. unutmayın polkadot cüzdanı lazım. yani bir cüzdana sahip olmamız lazım :D

![image](https://github.com/Core-Node-Team/Testnet-TR/assets/91562185/52029b07-2968-4495-a34c-8113b78cd865)

![image](https://github.com/Core-Node-Team/Testnet-TR/assets/91562185/788a4687-66f7-4dd4-8faf-1d6e42e32591)
