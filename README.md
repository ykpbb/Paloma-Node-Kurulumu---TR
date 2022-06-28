# Paloma-Node-Kurulumu---TR
Paloma Node Kurulumu Türkçe Kaynak

Explorer: https://paloma.explorers.guru/

## SİSTEM GEREKSİNİMLERİ
### Minimum Sistem Gereksinimleri
- 3x CPU; saat hızı ne kadar yüksek olursa o kadar iyi
- 4GB RAM
- 80GB Disk
- Kalıcı İnternet bağlantısı (testnette minimum 10Mbps trafik olacak - üretim için en az 100Mbps bekleniyor)

## Önerilen Sistem Gereksinimleri
- 4x CPU; saat hızı ne kadar yüksek olursa o kadar iyi
- 16GB RAM
- 200GB DİSK (SSD veya NVME)
- Kalıcı İnternet bağlantısı (testnette minimum 10Mbps trafik olacak - üretim için en az 100Mbps bekleniyor)

## MANUEL NODE KURULUMU
Full node kurulumu için aşağıdaki yönergeleri adım adım takip edin.

### Değişkenleri Ayarlama
Buraya, gezginde görünecek olan takma adınızı (doğrulayıcı) girmelisiniz.
```
NODENAME=<MONIKER_ADINIZ>
```
Değişkenleri sisteme kaydedin ve içe aktarın.
```
PALOMA_PORT=10
echo "export NODENAME=$NODENAME" >> $HOME/.bash_profile
if [ ! $WALLET ]; then
	echo "export WALLET=wallet" >> $HOME/.bash_profile
fi
echo "export PALOMA_CHAIN_ID=paloma-testnet-5" >> $HOME/.bash_profile
echo "export PALOMA_PORT=${PALOMA_PORT}" >> $HOME/.bash_profile
source $HOME/.bash_profile
```

## Paketleri Güncelleyin
```
sudo apt update && sudo apt upgrade -y
```

## Bağımlılıkları Yükleyin
```
sudo apt install curl build-essential git wget jq make gcc tmux -y
```

## Go Yükleyin
```
ver="1.18.2"
cd $HOME
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz"
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz"
rm "go$ver.linux-amd64.tar.gz"
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> ~/.bash_profile
source ~/.bash_profile
go version
```

## İkili Dosyaları İndirin ve Oluşturun
```
wget -O - https://github.com/palomachain/paloma/releases/download/v0.2.4-prealpha/paloma_0.2.4-prealpha_Linux_x86_64.tar.gz | \
sudo tar -C /usr/local/bin -xvzf - palomad
sudo chmod +x /usr/local/bin/palomad
sudo wget -P /usr/lib https://github.com/CosmWasm/wasmvm/raw/main/api/libwasmvm.x86_64.so
```

## Uygulamayı Yapılandırın
```
palomad config chain-id $PALOMA_CHAIN_ID
palomad config keyring-backend test
palomad config node tcp://localhost:${PALOMA_PORT}657
```

## Uygulamayı Başlatın
```
palomad init $NODENAME --chain-id $PALOMA_CHAIN_ID
```

## Genesis ve Addrbook İndirin
```
wget -O ~/.paloma/config/genesis.json https://raw.githubusercontent.com/palomachain/testnet/master/paloma-testnet-5/genesis.json
wget -O ~/.paloma/config/addrbook.json https://raw.githubusercontent.com/palomachain/testnet/master/paloma-testnet-5/addrbook.json
```

## Seeds ve Peers Ayarlayın
```
SEEDS=""
PEERS="fae84ec72a6f686d76096053e0532a65b69e5228@143.198.169.111:26656,3a06e1d98f831963a09a16561c4125e4eec5ed06@195.3.223.33:30656,f9d0db52347811f07b0aab4047099281ed042533@88.208.57.200:36656,e1efddf3b39f1953590f8264d30d71d1a1313061@164.90.134.139:26656,301938da656d6224fdd35f806b1d2b67d94d8d36@34.69.131.169:26656,5d8e547ebe3c6b62a043f52ffc379898ce3ef578@128.199.229.55:36416,3a06e1d98f831963a09a16561c4125e4eec5ed06@195.3.223.33:30656,eb33a25834f0368c91bdc33c6178efa45b48e15f@142.93.222.212:36416,22ce759d389de8c0ef14710916ddba05246bce31@35.232.220.104:26656,368f268011d047a25ba1e658b29d8d68695eaefe@20.56.69.130:26656"
sed -i -e "s/^seeds *=.*/seeds = \"$SEEDS\"/; s/^persistent_peers *=.*/persistent_peers = \"$PEERS\"/" $HOME/.paloma/config/config.toml
```

## Özel Bağlantı Noktalarını Ayarlayın
```
sed -i.bak -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:${PALOMA_PORT}658\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:${PALOMA_PORT}657\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:${PALOMA_PORT}060\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:${PALOMA_PORT}656\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":${PALOMA_PORT}660\"%" $HOME/.paloma/config/config.toml
sed -i.bak -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:${PALOMA_PORT}317\"%; s%^address = \":8080\"%address = \":${PALOMA_PORT}080\"%; s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:${PALOMA_PORT}090\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:${PALOMA_PORT}091\"%" $HOME/.paloma/config/app.toml
```

