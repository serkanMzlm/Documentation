# PX4 

```bash title="Kurulum ve Ön Ayarlar" linenums="1"             
# --recursive alt modüllerinin (submodules) de otomatik olarak klonlanmasını sağlar.
git clone https://github.com/PX4/PX4-Autopilot.git --recursive

# `--no-nuttx` nuttx yüklemesi kapatılmış olur
# `--no-sim-tools` simülasyon ortamı kapatılmış olur.
bash ./PX4-Autopilot/Tools/setup/ubuntu.sh
bash ./PX4-Autopilot/Tools/setup/ubuntu.sh --no-sim-tools --no-nuttx

# Ek bağımlılıklar
sudo apt-get install protobuf-compiler libeigen3-dev libopencv-dev -y

# Oluşabilicek hata durumunda 
git submodule update --recursive
make distclean

# region 'flash' overflowed by XXXX bytes yüklenen dosyanın 
# boyutu büyük olması durumunda karşılanır.
```

```bash title="" linenums="1"             
make [VENDOR_][MODEL][_VARIANT] [VIEWER_MODEL_DEBUGGER_WORLD]
```

- **VENDOR:** Kartın donanım üreticisini belirtir.
- **HARDWARE_MODEL:** Gerçek donanım kartı modelini seçer.
- **VARIANT:** Kartın belirli yapılandırmasını (bootloader, firmware çeşidi vb.) belirtir.
- **VIEWER:** Simülasyonu görsel olarak hangi simülatörde izleyeceğinizi seçer.
- **VEHICLE_MODEL:** Simülatör içinde kullanılacak araç tipini belirler.
- **DEBUGGER:** Hata ayıklama aracı tercihini yapar.
- **WORLD:** Simülasyonun geçirileceği çevre (dünya) dosyasını seçer.

```bash title="Simülasyon" linenums="1"                 
make px4_sitl gz_x500

# Simülasyon ortamı GUI açılmaz
HEADLESS=1 make px4_sitl gz_x500 

# ROS2 ile harbeleşmesini sağlar
MicroXRCEAgent udp4 -p 8888

# Kart üzerinden gelen verileri ROS2 olarak yayınlanması sağlanır.
sudo MicroXRCEAgent serial --dev /dev/AMA0 -b 921600
sudo MicroXRCEAgent serial --dev /dev/ttyUSB0 -b 921600

make px4_sitl list_vmd_make_targets  # Tüm seçenekleri listeler.

# Varsayılan değerler olduğu için bazı kısımlar _ ile boş bırakılabilir.
gazebo-classic___gdb = gazebo-classic_iris_gdb

make px4_fmu-v5_default               # Kodu derlenmesini sağlar.
make px4_fmu-v5_default upload        # Kodun karta yüklenmesini sağlar.

make list_config_targets             # Kartların listelenmesini sağlar.
make px4_fmu-v5_default boardconfig  # Paketlerde ayarlama yapılmasını sağlar.
git submodule update --init --recursive

CONFIG_EXAMPLES_PX4_SIMPLE_APP=y     # Manuel ayarlamaların yapılması
```

- `_default` kavramı isteğe bağlıdır. Bu kavram belirli özelliklerin desteklenmesi veya atlanması gibi bir ürün yazılımı yapılandırmasını belirtir.


## Komut Satırı Kullanımı

```bash title="" linenums="1"             
# Tüm parametreleri listeler
param show 

# Varsayılan değerlerini gösterir.
param show -c

# Belirli parametreyin değerini gösterir
param show RC_MAP_A*

# Parametrelerini kaydeder
param save

# Belirli bir konuma kaydeder
param save /fs/microsd/vtol_param_backup

param load
param import

# Parametreleri dosyanın kaydedildiği zamana sıfırla
param load /fs/microsd/vtol_param_backup
# İsteğe bağlı olarak parametreleri kaydedin (yükle birlikte otomatik olarak yapılmaz)
param save
```
