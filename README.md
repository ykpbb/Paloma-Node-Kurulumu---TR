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
