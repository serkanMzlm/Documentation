# Linux

## Temel Bilgiler
- **root:** Sistem yöneticisi, en yetkili kullanıcı. Komut satırı istemi `#` ile gösterilir.
- **Normal kullanıcı:** Standart yetkili kullanıcı. Komut satırı istemi `$` ile gösterilir.
- **Yorum satırı:** Satır başında `#` karakteri yer alırsa, kabuk (shell) bu satırı yorum olarak atlar.
- Dosya veya dizin isminin başında `.` olursa, Linux tarafından gizli kabul edilir. Örnek: `.config/`
- **Bağlantılar (Links):**
    - **Soft link (sembolik):** Başka bir dosyaya referans oluşturur. Gerçek dosyanın kopyası değildir.
    - **Hard link (sert):** Dosyanın tam içeriğine işaret eder; silinirse bile diğer link üzerinden erişilebilir.
- **Servisler ve İşlemler:**
    - **daemon:** Genellikle sistem başlatıldığında arka planda çalışan uzun ömürlü süreçlerdir. Örnek: sshd, cron
    - **process (işlem):** Bir uygulama veya komut çalıştırıldığında oluşan yürütülebilir birimdir. Kısa ömürlü olabilir.
- **Dosya Türleri ve İzinleri**
    - `d` : Dizin (directory)
    - `l` : Sembolik bağlantı (symbolic link)
    - `-` : Normal dosya
    - `c` : Karakter aygıtı (örneğin /dev/ttyUSB0)
    - `b` : Blok aygıtı (örneğin disk bölümü)
    - `s` : Socket
    - `p` : FIFO (named pipe)
    - `(rwx)`: Okuma (r=4), yazma (w=2), çalıştırma (x=1).

!!! note "örnek"

    `drwxr-xr--` dizin, sahibi tüm izinlere, grup okuma ve çalıştırma iznine, diğerleri sadece okuma iznine sahip.

| Terminal Kısayolları | İşlevi |
|------|------|
| `Alt + F2` | Komut çalıştırma penceresini açar (grafik ortam). |
| `Ctrl + C` | Çalışan komutu sonlandırır. |
| `Ctrl + R` | Önceki komutlarda arama yapar. |
| `Ctrl + S` | Terminal akışını durdurur (komut yürütme durmaz). |
| `Ctrl + Q` | Terminal akışını sürdürür. |
| `Ctrl + U` | İmlecin solundaki tüm metni siler. |
| `Ctrl + Z` | Çalışan komutu arka plana alır (durdurur). |

## Operatörler ve Yönlendirmeler

|Operatör | Açıklama |
|---------|----------|
| `;` | Komut ayracı. Birden fazla komutu sırayla çalıştırır: `komut1; komut2`. |
| `&` | Komutu arka planda çalıştırır: `sleep 10 &`. |
| `|` | Bir komutun çıktısını başka bir komutun girdisine (pipe) yönlendirir. |
| `*` | Sıfır veya daha fazla karakterle eşleşir (her şeyi temsil eder). |
| `?` | Tam olarak bir karakterle eşleşir. |
| `[...]` | Köşeli parantez içindeki karakterlerden herhangi biriyle eşleşir. Örneğin, `[ab]` sadece ‘a’ veya ‘b’ ile başlayan dosyaları listeler |
| `{x..y}` | Belirli bir aralıktaki sayıları veya karakterleri üretir. `file{1..5}.txt`|
| `^` | Düzenli ifadelerde satır başını ifade eder. |
| `>` | Standart çıktıyı (stdout) belirtilen dosyaya yazar, önceki içerik silinir. |
| `>>` | Standart çıktıyı dosyanın sonuna ekler. |
| `<` | Standart girdiyi (stdin) bir dosyadan alır. |
| `2>` | Standart hata çıktısını (stderr) belirtilen dosyaya yönlendirir. |
| `&>` | Hem stdout hem stderr’i aynı anda yönlendirir. |
| `tee` | Standart çıktıyı hem ekrana basar hem de dosyaya yazar. `-a` ile eklemeli yazma yapılır. |
| `` | |

```bash
# Arka planda 10 saniye uyur
sleep 10 &

# 1'den 7'ye kadar dosyalar oluşturur
touch a{1..7}.txt

# 'a' veya 'b' ile başlayan dosyaları listeler
ls [ab]*

# a'dan z'ye kadar tek karakterli isimleri listeler
ls [a-z]

# Yönlendirme örnekleri:
echo "hello world" > file_name.txt
ls >> file.txt
cat < file.txt
telnet localhost 2> errorfile.txt
echo "hello world" | tee -a file.txt
```

## Linux Dizin Yapısı

| Dizin | Açıklama |
|-------|----------|
| `/ (root)` | Tüm dosya sisteminin kök noktasıdır. |
| `/boot` | Önyükleme (boot) ile ilgili çekirdek ve başlangıç dosyaları. |
| `/dev` | Donanım aygıtlarını temsil eden özel dosyalar. |
| `/etc` | Sistem yapılandırma dosyaları. |
| `/bin, /sbin` | Temel kullanıcı ve sistem ikili dosyaları. |
| `/usr` | İkincil hiyerarşi: uygulamalar, kütüphaneler, belgeler. |
| `/opt` | Üçüncü taraf yazılımlar için isteğe bağlı paketler. |
| `/lib, /lib64` | Temel sistem kütüphaneleri. |
| `/tmp` | Geçici dosyalar. |
| `/home` | Kullanıcıların ev dizinleri. |
| `/var` | Değişken veri dosyaları (loglar, posta, spool dosyaları). |
| `/run` | Geçici çalışma süresi bilgisi (PID dosyaları, kilitler). |
| `/mnt, /media` | Harici medya ve dosya sistemleri bu noktalara bağlanır. |

!!! note "Not"
    Linux dosya sisteminde büyük/küçük harf duyarlılığı olduğunu unutmayın.


## Paket Kurulumu

Linux dağıtımlarında paket yönetimi için yaygın olarak **APT (Advanced Packaging Tool)** ve **DPKG** araçları kullanılır.

```bash title="apt paketleri" linenums="1"
# Paket listelerini günceller (depo bilgilerini yeniler)
apt update

# Yüklü paketleri en son sürümlere günceller (yeni paket kurmaz)
apt upgrade

# Yüklü paketleri ve gerektiğinde yeni paketleri de kurarak sistemin tamamını günceller
apt full-upgrade    # Debian/Ubuntu'da dist-upgrade eşdeğeri

# Yeni paket kurar
apt install <paket_adı>

# Paket kaldırır (ayar dosyaları korunur)
apt remove <paket_adı>

# Paket ve ilişkili yapılandırma dosyalarını siler
apt purge <paket_adı>

# Kullanılmayan bağımlılıkları kaldırır
apt autoremove

# İndirilmiş ve kullanılmayan .deb dosyalarını temizler
apt autoclean

# Kurulu tüm paketleri listeler
apt list --installed

# Hatalı bağımlılıkları düzeltir ve eksik paketleri kurar.
apt install -f

# Mevcut paket sürümünü yeniden yükler.
apt --reinstall install <paket_adı>

# Depo veritabanında paket araması yapar.
apt search <anahtar_kelime>

# Paket hakkında ayrıntılı bilgi görüntüler
apt show <paket_adı> 

# Depo listelerini düzenlemeye yarar (örn. sudo nano /etc/apt/sources.list).
apt edit-sources 
```

```bash title="dpkg" linenums="1"
# Kurulu tüm paketleri listeler
dpkg -l

# Yerel .deb dosyasını kurar (paketin .deb dosyasını belirtir)
dpkg -i <paket.deb>

# Yapılandırma adımı tamamlanmamış paketleri onarır
dpkg --configure -a

# Paket ve yapılandırma dosyalarıyla birlikte siler
dpkg -P <paket_adı>
```

## Sistem Günlükleri (System Logs)

Linux'ta sistem olayları, hata mesajları ve uyarılar genellikle `/var/log` dizininde depolanır. Ayrıca systemd kullanan sistemlerde `journalctl` ile günlüğe erişim mümkündür.

| Dosya / Komut | Açıklama |
|---------------|----------|
| `/var/log/boot.log` | Önyükleme sırasında oluşan mesajlar (init, kernel, servis bilgileri). |
| `dmesg` | Çekirdek (kernel) ring buffer’ını gösterir. |
| `/var/log/auth.log` | Kimlik doğrulama ve güvenlik olayları (giriş denemeleri, sudo kullanımı). |
| `/var/log/syslog` | Genel sistem ve uygulama mesajları (Debian/Ubuntu). |
| `/var/log/messages` | Genel sistem ve uygulama mesajları (Red Hat/CentOS gibi dağıtımlarda). |
| `/var/log/kern.log` | Kernel’e ait detaylı günlük kayıtları. |
| `journalctl` | systemd journal kayıtlarını okur |

```bash 
# Son önyüklemeden itibaren tüm logları gösterir
journalctl -b  

# Belirli bir servisin günlüklerini filtreler
journalctl -u <servis> 
```

!!! note "Not"
    Yoğun log akışını filtrelemek için grep veya less ile birlikte kullanabilirsiniz:
    ```bash
    journalctl -u sshd | grep "failed"
    dmesg | less
    ```

- `/etc/motd` **Message Of The Day**, Kullanıcı oturumu açtığında gösterilen kısa bilgilendirme mesajıdır.
- `/etc/motd` dosyasına yazdığın metin, her girişte terminale basılır.
- Dinamik içerik için `/etc/profile.d/` altına script ekleyebilirsin.

## Komutlar

### Dizin ve Dosya Gezinme
- `cd`  Dizinler arasında geçiş yapar. (`.` bulunduğun dizin, `..` bir üst dizin, `-` bir önce ki dizin, `~` home)
```bash
cd            # Home dizini
cd ~          # Home dizini
cd ../..      # İki üst dizin
```

- `pwd`  Mevcut dizinin tam yolunu gösterir.
- `ls`  Dizin içeriğini listeler.
- `tree`  Dizin yapısını ağaç formatında gösterir.

### Dosya ve Dizin İşlemleri
- `cp` Dosya veya dizini kopyalar.
```bash
cp -r <dir> <copy>  # Dizinleri ve içinde bulunan her şeyi kopylamayı sağlar. 
cp -l <file> <copy> # Kopyalama işlemini link şeklinde yapar.
```

- `mv` Dosyayı taşır veya yeniden adlandırır.
```bash
mv a.txt /home/       # Dosya taşıma
mv a.txt /home/b.txt  # Dosyayı taşırken isim değiştirme
mv a.txt b.txt        # Dosyanın ismini değiştirme
mv dizin /home/       # Dizinlerde taşınabilir.
```

- `rm` Dosya veya dizini siler. `!` ile dışında tutulabilir.
```bash
rm -rf                     # Belirtilen her şeyi siler
rm -i                      # Dosyaları sorarak siler.
rm -rf  !(a.txt)           # a.txt dışında dosyaları siler.
```

- `mkdir` Yeni dizin oluşturur.
```bash
mkldir -p dizin/dizin      # İç içe dizin  oluşturur.
```

- `rmdir` Boş dizini siler.
- `touch` Boş dosya oluşturur veya zaman damgasını günceller.
- `ln` Hard veya sembolik link oluşturur. `-s` sembolink link, `-i` hard link

### Metin Görüntüleme & İşleme
- `cat` Dosya içeriğini bir kerede gösterir.
```bash
cat a.txt                      # içeriği gösterir  
cat > a.txt                    # a dosaysını oluşturup içine yazı yazmamızı sağlarla
cat >> a.txt                   # a dosyası varsa üstüne yazmamızı sağlar.
cat a.txt >  b.txt             # a dosaysını b dosyasına kopyalar.
cat a.txt >> b.txt             # a dosaysını b dosyasının üstüne yazar.
```

- `more` Sayfa sayfa görüntüler. (Ekrana sığacak şekilde)
- `less` İleri/geri kaydırma ve arama imkânı sunar. `more` komutunun gelişmiş halidir.
```bash
less deneme.txt
__________
-N   : Her satırın başına numara koyar.
-i   : Büyük küçük harf duyarlılığını devre dışı bırakır.
-S   : Uzun satırları keserek , yatay kaydırma çıbuğu oluşturur.
-F   : Dosya içeriğini gösterdikten sonra otamatik olarak kapanır. 
__________
space: Tuşu bir sonraki sayfayı gösterir. 
b    : Bir önceki sayfayı gösterir.   
g    : Dosyanın başına gitmek için kullanılır.
G    : Dosyanın sonuna gitmek için kullanılır.
/    : Arama yapmak için kullanılır.
n    : Bir sonraki eşleşmeyi bulmak için kullanılır.
q    : less komutunu kapatmak için kullanılır.
__________
```

- `head / tail` Dosyanın baştan / sonundan N satırını gösterir.
- `grep - egrep` Metin içinde desen arar.
```bash
grep keyword file.txt  #Belirtilen kelimeyi dosya içinde arar
grep -c keyword file.txt #Belirtilen kelimenin dosyada kaç tane olduğunu gösterir.
grep -i keyword file.txt #Belirtilen kelimeyi dosya içinde arar. Arama yaparken büyük küçük duyarlılığı kapatır.
grep -n keyword file.txt # Hnagi satırlarada bulunaduklarını    gösterir.
grep -v keyword file.txt # Belirtilen anahtar kelimesi dışındaki yerleri alır
ls | grep Desktop
egrep -i "keyword1|keyword2" file.txt  # Arama yaparken keyword1 veya keyword2 birinin olması yeterli olur.
```

- `cut` Belirtilen sütunları veya karakterleri keser.
```bash
cut -c1 file.txt  # 1. sütünda bulunan karakterleri gösterir.
cut -c1,3,4,5 file.txt  # 1., 3., 4., 5. sütünda bulunan karakterleri gösterir.
cut -c1-4 file.txt  # 1. sütündan 4. sütüna kadar bulunan karakterleri gösterir.
cut -c1-4, 6-8 file.txt
cut -b1-3 file.txt   # 1. sütündan başlayıp 3 byte veri al
ls -la | cut -c2-4
```

- `awk` Alan bazlı metin işleme sağlar.
```bash
awk '{print $1}' file.txt  #Dosyanın içinde bulunan her satırda bulunan ilkkelimeyi gösterir.
awk '{print $2, $4}' file.txt 
awk '{print $NF}' file.txt #Her satırın son kelimesini gösterir. 
awk '/jer/ {print}' file.txt #Dosyanın içinde jey arar ve varsa ekranda gösterir. 
cat file.txt | awk '{$2="hi"; print $0}  #Bu dosaynın içinde bulunan her satırda 2. kelimesini "hi" ile değiştirir.
awk 'lenght($0) > 20' file.txt  #İlk karakteri 20 bytetan fazla olan satırları gösterir.
ls -l | awk '{if($9 == "Des") print $0;}'  #Şartı sağlayan satırı gösterir.
```

- `sed` Satır içi metin değiştirmeye yarar.
```bash
sed 's/[kelime]/[yeni_kelime]/g' dosya.txt   # kelime olan kısımlar  yeni_kelime ile değiştirilir.
sed -i 's/[kelime]/[yeni_kelime]/g' dosya.txt   #  -i parametresi sayesinde dosyanın içinde yapılır değişiklik
sed -i 's/[kelime]/d' dosya.txt   #  kelime siler
```

- `sort` Satırları belirli kritere göre sıralar.
```bash
sort file.txt  #Alfabetik sıraya göre sıralar
sort -r file.txt  #Alfabetik sıranın tersine  göre sıralar
sort -k2 file.txt  #Alfabetik sıraya göre sıralar (2. kelimeye bakarak sıralam yapılır)
```

- `uniq` Birbirini izleyen tekrarları filtreler.
```bash
uniq file.txt  #Aynı olan satıorların sadece birini gösterir
uniq -c  file.txt  #Her satırın kaç kez geçtiğini gösterir.
sort file.txt | uniq
sort file.txt | uniq -c
sort file.txt | uniq -d   #Sadece tekrarlanan kısmı gösterir.
```

- `tr` Karakter dönüşümleri yapar.
```bash
cat a.txt | tr ‘a-z’  ‘A-Z’              # Bütün harfleri büyük yazar
```

- `wc` Satır, kelime ve byte sayar.
```bash 
wc file.txt # satır kelime byte çıktısı verir
wc -l file.txt # satır sayısını verir.
wc -w file.txt # kelime sayısını verir.
wc -c file.txt # byte sayısını verir.
ls -l | wc -l
grep  keyword file | wc -l   #belirtilen keyword kaç kez geçtiğini bulmuş oluruz
```

- `diff` Dosyalar arasındaki satır farklılıklarını gösterir. (line by line)
- `cmp` Dosyaları byte byte karşılaştırır. (byte by byte)
- `truncate` kesme işlemi yapar.
```bash
truncate -s 40 file.txt  #dosyayı 40 byte olarak keser fazlalık olan kısmı siler. Sıkıştırma işlemi yapmaz.
```

- `file` Dosya hakkında bilgi verir. `file a.txt`
- `find - locate` Dosya araması yapar.

!!! note  "Not" 
    find ile locate komutları aralarında ki fark **locate** komutunun bir veri tabanı vardır. Bu yüzden aramaları daha hızlı yapar. Fakat bu veri tabanı güncellenmez ise yeni oluşturulan dosyaları bulamayız.
```bash
find . -size +100M        # 100mb büyük dosyaları arar
find . -type f -perm 0777 # Belirtilen izinlere sahip dosyaları bulur.
find . --name *.gz        # gz uzantılı dosyaları bulur.
find . -mtime 3           # 3 gün içinde değişiklik yapılan dosyaları bulur.

locate a.txt
updatedb                  # locate komutunun datebase günceller.
```

- `readlink`  Bu komut sayesinde link edilmiş bir dosyanın gerçek konumunu gösterir.

### Süreç & Kaynak Yönetimi
- `ps` Çalışan süreçleri listeler.
    - TTY sütunu, işlemin hangi terminalde yürütüldüğünü,
    - TIME sütunu, o işlemin ne kadar süredir çalıştığını,
    - CMD sütunu ise hangi komutun çalıştırıldığını gösterir.
    - `ps aux` çıktısındaki STAT sütununda yer alan harflerin anlamları ise şunlardır:
        - **D:** uyku modu (disk I/O için bekliyor)
        - **R:** çalışıyor
        - **T:** duraklatılmış
        - **X:** zorla durdurulmuş
        - **Z:** zombi işlem
        - **S:** işlem için kaynak (örneğin CPU) bekliyor
- `pstree` ise bu işlemleri adeta bir aile ağacı gibi detaylı şekilde görselleştirir—kim kimin çocuğu, kim kimin ebeveyni, hepsini tek bakışta anlarsınız!
```bash
ps 
ps -a
ps -ef
pstree

# En fazla CPU tüketen 5 uygulamayı listeler
ps H -eo pid,pcpu | sort -nk2 | tail

# PID karşılık Gelen uygulamanın hangisi olduğu anlaşılır.
# ll ilede gösterilebilir.
ps aux | fgrep [PID] 
ll /proc/[PID]
```

- `top` Canlı sistem kaynak kullanımını gösterir. PID, USER, PR(öncelik seviyesi), RES(RAM miktarı) gibi durumlar gözlenebilir.
```bash
top -u [user_name]   # Belirli bir kullanıcının detaylı gösterilmesini sağlar.
_____________________________
k : ile belirli bir görevi direkt top komutu üzerinden kill yapılması sağlanır.
```

- `kill` Süreci sonlandırmak için sinyal gönderir.
```bash
ps -a                  # Tüm işlemler
ps aux
ps -e                  # O kullanıcı tarafından yapılan tüm işlemler
ps -u [user_name]      # Belirli bir kullanıcının yaptıklarını gösterir.
kill -l                # Sinyallari sitler.
kill -9 PID_number     # İşlemi sonlandırır.
kill -1 PID_number     # İşlemi yeniden başlatır.
killall       top      # PID numarası yerine direkt işlemin adı verilir.
```

- `nohup` Komutu oturumdan bağımsız çalıştırır. Yani terminal kapansa dahi komut yürütülmeye devam eder.
```bash
# nohup process &
# nohup process > /dev/null 2>&1 &
nohup sleep 75 &
nohup sleep 70 > /dev/null 2>&1 &
```

- `bg` Durdurulan işlemi arka plana alır.
- `fg` Arka plandaki işlemi ön plana getirir.
- `jobs` O anki kabuk işlerinin durumunu listeler.
- `nice - renice` Komuta öncelik (niceness) atar. / Çalışan sürecin önceliğini değiştirir.

- `script` Terminal oturumunu kaydeder.
- `timeout` Komutun çalışmasını süre ile sınırlar.
```bash
timeout 10s ping google.com
timeout 1m ping google.com
timeout 1h ping google.com
```

- `dd` “Disk Dump” kısaltmasıyla bilinir. Unix/Linux’ta dosya, disk ve blok düzeyinde kopyalama yapmanızı sağlar. Veri bloklarını farklı biçimlerde işleyerek yedekleme, klonlama veya sıfırlama (örneğin `/dev/zero` kullanarak) gibi işlemlerde kullanılabilir.
```bash
dd if=/dev/sda of=/mnt/backup/sda.img bs=64K conv=noerror,sync
```

### Sistem Bilgisi & İstatistik
- `uname` Sistem bilgilerini görüntüler.
```bash
uname -a    # Tüm sistem bilgileri
uname -s    # İşletim sisteminin adı
uname -r    # İşletim sisteminin sürümünü gösterir.
```

- `arch` İşletim sistemi mimarisini gösterir. (64 / 32)
- `df` Dosya sistemi kullanım özetini verir.
- `du` Dizin veya dosya boyutunu hesaplar.
```bash
du dosya_yolu
du -h dosya_yolu    # Boyutları okunabilirliğini artırır.
du -sh dosya_yolu   # Alt dizinleri almaz
```

- `free` Bellek (RAM) kullanımını gösterir.
```
free 
cat /proc/cpuinfo   # Tüm sistem bilgilerinin bulunduğu dosyadır.
cat /proc/meminfo   # RAM hakkında bilgi verir.
```

- `dmesg` Kernel mesajlarını okur.
```bash
dmesg
dmesg | less
dmesg -H  #Anlaşılır
dmesg -c  #Günlüğü temizler
dmesg -l  #Günlükleri filtrelemek için kullanılı (-l err sadece hataları gösterir)
```

- `lsmod` Yüklü çekirdek modüllerini listeler.
- `lsblk` Blok aygıtlarını ağaç yapısında gösterir.
- `fdisk` Disk bölüm bilgilerini yönetir.
- `blkid` Blok aygıtlarının UUID ve tür bilgilerini listeler.
- `badblocks` Bozuk disk bloklarını tarar.
- `fsck` Dosya sistemini onarır.

### Ağ & İnternet
- `ping` Ağ bağlantısını test eder. `-c` ile kaç kez atılacağı ayarlanır.
- `ifconfig`p addr — Ağ arayüzü yapılandırmasını gösterir.
- `netstat - ss` Aktif bağlantıları listeler.
- `curl` URL’lerden veri transferi yapar.
- `wget` Dosya indirir.
```bash
wget https:/...../.....tar.gz
curl -o putty httğs:/..../...tar.gz
```

- `dig - nslookup` Komutları, belirli bir alan adı veya IP adresi hakkında bilgi almak için DNS (Domain Name System) sunucularına sorgu yapmak için kullanılan komut satırı araçlarıdır.
- `iptables` Paket filtreleme ve NAT kurallarını yönetir.
- `arp` ARP tablosunu gösterir.
- `hostname` Sistem adı ve IP adresini verir. Sistemin ağ üzerindeki adını (hostname) `/etc/hostname` dosyasından okur ve değiştirir `sudo hostnamectl set-hostname yeni_hostname`
- `netdiscover` Ağ taraması yapar.

- `nmcli` NetworkManager’ı komut satırı üzerinden yönetmenizi sağlayan güçlü bir araçtır. Ağ bağlantıları oluşturabilir, düzenleyebilir, silebilir ve mevcut durum hakkında ayrıntılı bilgi alabilirsiniz.

```bash
nmcli               # Genel durum bilgisi
nmcli connection show   # Tüm bağlantıları listeler
nmcli device status     # Ağ arayüzlerinin durumunu gösterir
nmtui
nmcli’nin metin tabanlı, kullanıcı dostu arayüzüdür. CLI yerine basit menülerle ağ ayarlarını yönetmek isterseniz tercih edebilirsiniz.
```

- `nm-connection-editor` NetworkManager bağlantılarını grafiksel (GUI) ortamda düzenlemenizi sağlar. Masaüstü kullanıcıları için idealdir.



### Kullanıcı & İzinler
- `chmod` Dosya/dizin izinlerini değiştirir. (u = user, g = groups, o = or)
- `chown` Sahiplik bilgilerini değiştirir.
- `chgrp` Grup bilgisini değiştirir.
```bash
chmod 777 file
chmod ugo+rwx file
chmod g-x file
chmod +x file

chown root file
chgrp root file

chown yeni_kullanıcı a.txt
chgrp grupadi dosyaadi
```

- `useradd` Yeni kullanıcı ekler.
- `usermod` Mevcut kullanıcıyı düzenler.
- `userdel` Kullanıcı siler.
```bash
userdel -r user_1  # Kullanıcıyı home dizini ile beraber siler
usermod -U user_1  # Aktif 
usermod -L user_1  # Pasif
```

- `groupadd` Yeni grup oluşturur.
- `groupdel` Grup siler.
- `passwd` Kullanıcı şifresi ayarlar.
- `chfn` Kullanıcı bilgilerini değiştirir.
- `getfacl`etfacl — ACL bazlı izinleri yönetir.
- `id` Kullanıcının bağlı olduğu ID gösterir.

- `w` Sistemde çevrim içi kullanıcı sayısını gösterir.
- `lastb - last` lastb Sisteme başarısız girişleri gösterir. last bütün girişleri gösterir
- `users`  Sistemde aktif kullanıcıları gösterir.
- `whoami`  O an ki kullanıcı adını verir.

### Zaman & Planlama
- `date` Tarih ve saati gösterir veya ayarlar.
```bash
date                                # Tarih ve saati gösterir
sudo date --set="$(date)"           # Tarih ve saati düzenler
sudo date -s "10 MAR 2023 10:19:23" # Tarih ve saati düzenler
sudo date -s "2023-12-16 09:16:00"
sudo date +%T -s "13:30:00"
# Ya da 
timedatectl set-time '2015-11-20 22:13:20'
```

- `sleep` Belirtilen süre bekler. `sleep 10`
- `watch` Komutu belirli aralıklarla tekrarlar.
- `crontab` Zamanlanmış görevler oluşturur.
- `at` Tek seferlik zamanlanmış iş başlatır.

!!! note "Not"
    Linux sisteminde belirli zaman aralıklarında veya zamanlamalar da tekrarlanması gereken görevleri otomatikleştirmek için kullanılır. crontab komutu belirli günlerde belirli haftalarda belirli zaman aralıklarında tekrarlamasını istediğimiz komutlar için kullanılır. Oluşan dosyalar `etc/cron.[]` konumunda bulunur. `crontab -e`  crontab dosyasını düzenlemek için kullanılır. Açılan dosyananın içine dakika saat gün ay ve haftanın günü son olarakda yapılacak komut yazılır.
    ```bash
    crontab -e
    0 2 * * * echo "hello world" > a.txt   # Bu kısımda * işareti o kabsadığı alanın her zaman dilimini alır.

    at now + 1 hour echo "hello world"   # ctrl + D işlemi tamamlanır.
    ```

- `uptime` Çalışma süresi ve kullanıcı sayısını gösterir.

### Arşivleme & Sıkıştırma
- `tar` Arşiv oluşturur veya açar.
- `gzip - unzip` Dosya sıkıştırır veya açar.
```bash
tar -cvf a.tar .  # O anki dizini a.tar içine kopyalar.
gzip a.tar        # a.tar dosaysını a.tar.gz olarak sıkıştırır.
gzip -d a.tar.gz  # dosyayı açar 
gunzip  a.tar.gz  # dosyayı açar
```
- `zip`nzip — Zip arşivleriyle çalışır.
- `truncate` Dosyayı belirtilen boyuta keser.
```bash
truncate -s 40 file.txt  #dosyayı 40 byte olarak keser fazlalık olan kısmı siler. Sıkıştırma işlemi yapmaz.
```

### Kabuk & Yardım
- `reboot` Sistemi yeniden başlatır.
- `poweroff` Sistemi anında kapatır.
- `shutdown`  Sistemi kapatırken seçenekler sunar.
- `exit` Açık olan kullanıcıdan çıkar.
- `env - printenv` Sistemde bulunan evrensel değişkenleri gösterir. Ortam değişkenini kullanmak için `$` kullanılır
```bash
env
printenv
echo $PATH
```

- `su` Kullanıcı değiştirmeyi sağlar.
```bash
su user_1   # user_1 kullanıcısına geçer
sudo su     # root kullanıcısına geçer
sudo -i     # root kullanıcısı olarak yeni bir kulanıcı kabugu açılmış gibi davranır.
sudo -s     # o an ki kabugun root gibi davranmasını sağlar.
```
- `bash` Yeni bir kabuk başlatır.
- `source` Betik dosyasını mevcut kabuğa yükler.
- `clear` Terminali temizler
- `history` Kabuk komut geçmişini listeler.
```bash
history
HISTSIZE=2000      # history komutunda en fazla kaç komut tutulacağını ayarlar.
!124               # history komutunda 124 sırada komut çalışır.
!!                 # Bir önceki komut çalışır
!-3                # Üç önceki komut çalışır.
```

- `man` Komut kılavuz sayfasını açar.
- `info` GNU bilgi sayfasını gösterir.
- `--help` Komut yardımını ekrana basar.
- `alias - unalias` Takma ad oluşturur veya siler.
- `apropos` Belirtilen anahtar kelimeye göre kılavuz sayfalarını arar.
```bash
apropos "list directory contents"
```

- `export` Ortam değişkeni tanımlar.
- `lsb_realease`  Kullanılan sürüm hakkında bilgi verir.
- `who`  Sisteme giriş yapmış kullanıcıları gösterir.
- `whatis`  Komut hakkında kısa bilgi verir.
- `whereis / which`  Komutun konumunu gösterir.
- `wall` Tüm kayıtlı kullanıcılara terminal üzerinden mesaj iletmenizi sağlar. Yalnızca yetkili kullanıcılar tarafından çalıştırılabilir.
```bash
wall "Sistem bakımı yapılacaktır. Lütfen oturumunuzu kaydedin ve bekleyin."
```

- `write` Belirli bir kullanıcıya doğrudan mesaj göndermek için kullanılır. İleti girdisini tamamlamak için Ctrl +D tuşuna basmanız yeterlidir.
```bash
write user_1
# Mesajınızı yazın, ardından Ctrl + D ile gönderin
```
### ACL (Access Control List)
Standart Unix izinlerinin ötesinde, her dosya veya dizine kullanıcı ve grup bazlı ayrıntılı erişim hakları tanımlamanıza imkân verir. Böylece belirli bir kullanıcının veya grubun okuma, yazma, çalıştırma gibi izinlerini hassas biçimde ayarlayabilirsiniz.

- `getfacl` Dosya/dizin üzerindeki ACL girişlerini ve temel izinleri gösterir.
- `setfacl` ACL üzerinden izin ekleme, düzenleme veya silme işlemleri yapar.

## Run Levels
Linux’un geleneksel SysV init sistemi, farklı işletim durumu “seviyelerini” (run levels) rakamlarla tanımlar. Her run level, hangi servislerin ve işlevlerin aktif olacağını belirler:

| Run Level	| Anlamı | 
|-------|-------|
| 0	 | Kapatma (shutdown). | 
| 1	 | Tek kullanıcı modu (sistem bakım/onarım). | 
| 2	 | Çoklu kullanıcı modu, ağ hizmeti kapalı (dağıtıma göre değişir). | 
| 3	 | Çoklu kullanıcı modu, ağ hizmeti aktif. | 
| 4	 | Tanımlanmamış / Kullanılmaz. | 
| 5	 | Çoklu kullanıcı + grafik arayüzlü (X11/Wayland) + ağ hizmeti. | 
| 6	 | Yeniden başlatma (reboot). | 

```bash
# Run level değiştirme (örnek: seviye 3)
sudo init 3
```

!!! note "Not"
    Modern dağıtımlarda Systemd kullanılıyorsa systemctl isolate multi-user.target veya graphical.target komutlarını tercih etmek daha güncel bir yöntemdir.


## Device Tree
Gömülü Linux sistemlerinde (örn. ARM tabanlı kartlar), donanımı çekirdeğe tanıtmak için Device Tree kullanılır.
- `.dts` (Device Tree Source): İnsan tarafından okunabilir metin formatı.
- `.dtb` (Device Tree Blob): Derlenmiş, ikili format.
1. Bir sensör, kontrolcü veya başka bir aygıt için özellikler .dts dosyasında tanımlanır.
2. Kernel açılırken bu veriyi okur, uygun sürücüyü yükler ve donanımı yapılandırır.
3. Cihazını Linux’e “tanıştırmanın” en düzenli yolu: Device Tree!

## Driver (Sürücü)
Linux çekirdeği ile donanım arasında köprü kuran yazılım bileşenidir. Donanımı algılar, kontrol eder ve veri akışını yönetir. Yeni bir cihaz eklediğinde, ilgili sürücüyü kernel modülü (ör. *.ko) olarak yüklersin.

## Shell Script

- Linux’ta yaygın olarak Bash kullanılır; yüklü tüm shell’leri görmek için: `cat /etc/shells`
- Script’in hangi shell ile çalışacağını en başta belirtmek için **shebang** kullanılır: `#!/bin/bash`
- Satır başına # koyarak o satırı yorum (açıklama) haline getirirsiniz.

!!! note "Not"
    Shebang `(#!/bin/bash)` da aslında yorum satırı gibi davranır ama kernel’e hangi yorumlayıcıyı yükleyeceğini söyler.

```bash
#!/bin/bash 
# read, echo, değişkenler    
my_host=`hostname`   # Bilgisayarın hostname verisini alır
name="Arif"
name2=arif           # İki değişken atamasıda doğrudur.

echo "server name: $my_host"   # Ekrana çıktı vermeyi sağlar.
echo "My name is $name."       # Bir değişkene erişmek için $ kullanılır.   
echo what is your name?
read new_name                 # Kullanıcdan veri alınması sağlanır.
read new_surname              # Bu olay read name surname olarak tek seferde
                              # alınabilir. Bir bolukla değişkenler doldurulur.
echo hello $new_name
```

| Operatör | 	Anlamı	| Örnek | 
|-------|-------------|---------|
| -eq | 	eşit	| `[ $a -eq 5 ]` | 
| -ne | 	eşit değil | 	`[ $a -ne 0 ]` | 
| -lt | 	küçük |	`[ $a -lt 10 ]` | 
| -gt | 	büyük |	`[ $a -gt 100 ]` | 
| -le | 	küçük veya eşit |	`[ $a -le 20 ]` | 
| -ge | 	büyük veya eşit	| `[ $a -ge 1 ]` | 
| -e | 	dosya veya dizin var mı	 | `[ -e /path/to/file ]` | 
| -f | 	normal dosya mu? |	`[ -f /etc/passwd ]` | 
| -d | 	dizin mi? |	`[ -d /home/user ]` | 

```bash
#!/bin/bash
gun=$(date +%a)  # Mon, Tue, …

if [ "$gun" = "Mon" ]; then
  echo "Bugün Pazartesi."
elif [ "$gun" = "Tue" ]; then
  echo "Bugün Salı."
else
  echo "Hafta içi başka bir gün."
fi

for i in 1 2 3 4 5; do
  echo "Sayaç: $i"
done

for num in {1..10}; do
  echo "Numara = $num"
  sleep 1
done

count=0
while [ "$count" -lt 5 ]; do
  echo "Merhaba Dünya ($count)"
  ((count++))
  sleep 1
done

echo "Seçiminiz (a: ls, b: cd ~, c: echo):"
read choice

case "$choice" in
  a) ls ;;
  b) cd ~ ;;
  c) echo "Hello, world!" ;;
  *) echo "Geçersiz seçim!" ;;
esac
```

## Ağ 
### IP Adresleri ve Sınıfları
Ağ üzerinde iletişim kuran her cihaza atanan benzersiz 32‑bit (IPv4) veya 128‑bit (IPv6) sayıdır. IPv4’te dört ondalık bölümle ***(ör. 192.168.1.10)***, IPv6’da ise sekiz grup onaltılık sayıyla **(2001:0db8:85a3::8a2e:0370:7334)** gösterilir.
- **IP (Internet Protocol):** İnternetteki her cihazın benzersiz kimliğidir. Trafiğin doğru hedefe yönlendirilmesini sağlar.
- **Sınıflar:** Eskiden her ip adresi 5 sınıfa ayrılırdı. Günümüzde CIDR yaygın olsa da temel fikir halen öğretici:

| Sınıf	| Aralık	| Kullanım Alanı | 
|-------|-----------|-----------------|
| A |	0.0.0.0   - 127.255.255.255 |	Çok büyük ağlar | 
| B |	128.0.0.0 - 191.255.255.255 |	Orta boy ağlar | 
| C |	192.0.0.0 - 223.255.255.255 |	Küçük ağlar | 
| D |	224.0.0.0 - 239.255.255.255 |	Multicast yayınlar | 
| E |	240.0.0.0 - 255.255.255.255 |	Deneysel / rezerve | 

- **Loopback:** 127.0.0.1 “kendi kendinle konuşman” için. Yerel sistem testi ve geliştirme için vazgeçilmez.

### Subnet Mask (Alt Ağ Maskesi)
- IP adresini **“ağ kısmı”** ve **“cihaz kısmı”** olarak ikiye bölen 32‑bit değer. Genellikle dört ondalık bölümle gösterilir (ör. 255.255.255.0). **255** olan bitler → ağ adresini, **0** olan bitler → host (cihaz) adresini temsil eder
- Örnek: 255.255.255.0 → ilk 24 bit ağ, son 8 bit host.
- CIDR karşılığı: /24

```bash
ip addr show eth0
# Çıktıda "inet 192.168.1.10/24" görürseniz:
#   - IP: 192.168.1.10
#   - Alt ağ masking: /24 (255.255.255.0)
```

### Gateway (Varsayılan Ağ Geçidi)
- Yerel ağdan çıkış noktasıdır. Genellikle bir router veya firewall cihazı.
- Farklı ağları birbirine bağlar.
- Bir cihaz başka ağdaki IP’lere paket gönderdiğinde kullanılır.
```bash
ip route
# default via 192.168.1.1 dev eth0 (Burada 192.168.1.1 sizin gateway’inizdir.)
```

### MAC Adresi
- Her NIC (Network Interface Card) için üretici tarafından atanan 48‑bit benzersiz donanım adresi. Genellikle altı çift onaltılık sayı ile gösterilir (00:1A:2B:3C:4D:5E).
- Veri Bağlantı Katmanı (OSI Katman 2).
- Donanıma gömülü, normalde değiştirilemez (ancak ip link set dev eth0 address ile yazılımla da değiştirilebilir).
- IP adresi katmanından bağımsız çalışır; LAN içi iletişimde kullanılır.
```bash
ip link show eth0
# link/ether 00:1a:2b:3c:4d:5e brd ff:ff:ff:ff:ff:ff
```

### DNS (Domain Name System)
- Alan adlarını (ör. chat.openai.com) IP adreslerine çeviren hiyerarşik dağıtık bir sistem.
- “İnsan-dostu” isimlerle internette gezinmeyi mümkün kılar.
```bash
dig +short example.com
# ya da
nslookup example.com
```

### NIC (Network Interface Card)
- Ağa bağlı cihazlar ve bilgisayarlar arasındaki veri iletişimini yönetmek için kullanılan bir donanım bileşenidir. NIC, bir bilgisayarın ağa bağlanabilmesi için gereken fiziksel bağlantıyı sağlar ve verilerin kablosuz veya kablolu ağlar üzerinden transfer edilmesini sağlar
- Fiziksel veya sanal ağ adaptörü.
- Ethernet, Wi‑Fi, Bluetooth vs. gibi farklı türleri vardır.

!!! note "Not"
    `/etc/hosts` Küçük ölçekli isim‑adres eşleştirmelerini içerir. 

### FTP (File Transfer Protocol)
- Port 21 üzerinden çalışan eski işlevsel dosya aktarım protokolü. Şifrelenmemiştir, bu yüzden güvensiz kabul edilir.
```bash
ftp ftp.example.com
# kullanıcı ve şifre ile oturum açarsınız
```

### Rsync
- Dosya ve dizinleri hem yerelde hem de uzak sistemlerde senkronize eden, delta-transfer (sadece değişen blokları) destekli araç.
    - `-a` Arşiv modu (izinler, tarih, vs.)
    - `-v` Ayrıntılı çıktı
    - `-z` Sıkıştırma
    - `--progress` Aktarım durumu
    - `--delete` ile hedefde olmayan dosyaları siler.
    - `--dry-run` ile neler yapılacağını test edersiniz.

```bash
rsync -avz --progress /local/dir/ user@remote:/backup/dir/
```

### SCP (Secure Copy Protocol)
- SSH tüneli üzerinden dosya transferi yapan, verileri otomatik olarak şifreleyen araç.
- `-r:` Dizinleri recursive kopyalar.
```bash
scp -r ./project user@192.168.1.100:/var/www/html/
```

### SSH (Secure Shell)
- Uzak makinaya şifreli bağlantı kuran, komut satırı ve port yönlendirme gibi özellikler sunan protokol. `ssh user@remote_host`
- Önemli Ayarlar (/etc/ssh/sshd_config):

| Ayar |	Açıklama | 
|-----|--------------|
| Port	| SSH dinleme portu (varsayılan 22). | 
| PermitRootLogin no	| Direkt root girişini engeller. | 
| PermitEmptyPasswords	| Boş parola kullanımını yasaklar. | 
| AllowUsers serkan	| Sadece belirtilen kullanıcı(lar) giriş yapabilir. | 
| ClientAliveInterval	| Boşta kalındığında sunucudan istemciye ping gönderme aralığı (s). | 
| ClientAliveCountMax	| Kaç ping sonrası bağlantıyı keser (0 = sınırsız). | 

```bash title="Anahtar Tabanlı Kimlik Doğrulama"
ssh-keygen -t ed25519     # Anahtar çifti oluşturur
ssh-copy-id user@host     # Public anahtarı uzak sunucuya kopyalar
ssh -i ~/.ssh/id_ed25519 user@host  # Belirli anahtar ile bağlanma
```
### Cockpit
- Web tabanlı, interaktif sunucu yönetim konsolu. Sistem durumu, servisler, konteynerler ve kullanıcı yönetimi gibi işlemleri tarayıcıda yapmanızı sağlar.

### DHCP (Dynamic Host Configuration Protocol)
- LAN’a katılan cihazlara otomatik IP, subnet mask, gateway ve DNS bilgisi atayan protokol.

## Communication
### Netlink Soketleri
- Linux çekirdeği ile kullanıcı alanı (user‐space) uygulamaları arasında iletişim sağlar.
- **AF_NETLINK** (veya **PF_NETLINK**) adres ailesini kullanır.
- Yaygın mesaj tipleri: **NETLINK_ROUTE** (ağ yönlendirme), **NETLINK_NETFILTER** (iptables), **NETLINK_KOBJECT_UEVENT** (udev olayları) vb.
- Mesaj yapısı:
```c
struct nlmsghdr {
    __u32 nlmsg_len;   /* Başlık + veri uzunluğu */
    __u16 nlmsg_type;  /* Mesaj tipi */
    __u16 nlmsg_flags; /* Bayraklar (örn. NLM_F_REQUEST) */
    __u32 nlmsg_seq;   /* Sıra numarası */
    __u32 nlmsg_pid;   /* Kaynak PID */
};

int sock = socket(AF_NETLINK, SOCK_RAW, NETLINK_ROUTE);
```

### ioctl (Input/Output Control)
- Cihaz sürücüleriyle veya özel dosya tabanlı arayüzlerle kontrol komutları göndermek için kullanılan bir sistem çağrısıdır.

### Aygıt Dosyaları (Device Files)
- `/dev` altında bulunan, fiziksel veya sanal donanım aygıtlarını temsil eden özel dosyalardır.
- Her bir aygıt dosyası, major (sürücü) ve minor (aygıt) numaralarıyla eşlenir.

### Sistem Çağrıları (System Calls)
- Kullanıcı alanındaki uygulamaların çekirdek (kernel) fonksiyonlarını çalıştırmasını sağlayan temel arayüzdür.
- Örnekler:
    - Dosya işlemleri: `open(), read(), write(), close()`
    - Süreç kontrolü: `fork(), execve(), waitpid()`
    - Bellek yönetimi: `mmap(), brk()`
    - Ağ iletişimi: `socket(), bind(), connect(), send(), recv()`
- Uygulamalar genellikle glibc gibi kütüphaneler üzerinden dolaylı çağrı yapar; nihai iş syscall talebiyle çekirdeğe düşer.
- **POSIX** uyumluluğu sayesinde, farklı Unix türevi sistemlerde benzer arayüzler kullanılır.

## Systemd
- Systemd, modern Linux dağıtımlarında “init” sisteminin yerini alan bir sistem ve hizmet yöneticisidir.
- Amaçları:
    - Paralel başlatma ile açılış süresini kısaltmak.
    - Bağımlılık grafiği kullanarak servisler arası doğru sırayı garantilemek.
    - Journald ile merkezi günlük kaydı (log) tutmak.
    - Socket, D-Bus, timer, mount ve daha pek çok birimi yönetmek.
-Genel Konumlar
    - Sistem geneli: `/etc/systemd/system/`
    - Dağıtım birincil: `/lib/systemd/system/` veya `/usr/lib/systemd/system/`
    - Kullanıcı bazlı: `~/.config/systemd/user/`
- Her `.service` dosyası üç ana başlığa sahiptir:
```service
[Unit]
Description=Kısa ve öz açıklama
After=network.target
Wants=postgresql.service

[Service]
Type=simple
ExecStart=/usr/bin/my-daemon --option
ExecReload=/bin/kill -HUP $MAINPID
Restart=on-failure
RestartSec=5s
User=myuser
Environment=ENV_VAR=value

[Install]
WantedBy=multi-user.target
```

- `[Unit]`
    - **Description:** Hizmetin ne yaptığını özetler.
    - **After / Before:** Başlama sırasını ayarlar. Örneğin **After=network.target → ağ** hazır olmadan başlamaz.
    - **Wants / Requires:** Zayıf / güçlü bağımlılık ilişkileri kurar.

- `[Service]`
    - **Type:** Başlatma davranışını belirler.
        - **simple (default):** ExecStart arka planda ayrılmadan çalışır.
        - **forking:** Daemon arka plana fork ettiğinde kabul edilir.
        - **oneshot:** Tek seferlik kısa işler için.
        - `notify, dbus, idle vb.` gelişmiş tipler de var.
    - **ExecStart, ExecReload, ExecStop:** Başlatma, yeniden yükleme ve durdurma komutları.
    - **Restart:** Yeniden başlatma koşulları (no, on-success, on-failure, always vb.).
    - **RestartSec:** Yeniden başlatma öncesi bekleme süresi.
    - **User / Group:** Hizmeti hangi kullanıcı/grup altında çalıştıracağını belirler.
    - **Environment:** Çevresel değişkenleri tanımlar.
    - **WorkingDirectory:** Çalışma dizinini ayarlar.

- `[Install]`
    - **WantedBy:** Hangi target’a “enable” edildiğinde ekleneceğini tanımlar. Örneğin `multi-user.target → systemctl enable my.service ile /etc/systemd/system/multi-user.target.wants/my.service` bağlantısı oluşturulur.
    - **RequiredBy:** Zorunlu alt birim ilişkisi oluşturur

### systemctl Komutları
```bash
sudo systemctl start   my.service   # Başlatır
sudo systemctl stop    my.service   # Durdurur
sudo systemctl restart my.service   # Yeniden başlatır
sudo systemctl status  my.service   # Durum bilgisini gösterir

sudo systemctl enable  my.service   # Boot’ta otomatik başlayacak şekilde işaretler
sudo systemctl disable my.service   # Artık otomatik başlamaz
sudo systemctl is-enabled my.service  # Etkin mi diye kontrol eder

sudo systemctl daemon-reload # Yeni veya güncellenmiş birim dosyalarını tanıtmak

systemctl list-units --all # Tüm birimleri listeler
systemctl list-units --type=target  # Sadece target’ları gör

# Sistemi kapat / yeniden başlat / uyku moduna al
sudo systemctl poweroff
sudo systemctl reboot
sudo systemctl suspend
sudo systemctl hibernate
```

```
[Unit]
Description=ROS 2 Startup Service
After=network.target

[Service]
Type=simple
User=rosuser
Environment=HOME=/home/rosuser
ExecStart=/home/rosuser/start_ros2.sh
Restart=on-failure
RestartSec=10s

[Install]
WantedBy=multi-user.target
```

- Bu dosyayı `/etc/systemd/system/ros2-start.service` olarak kaydedin.

```bash 
sudo systemctl daemon-reload
sudo systemctl enable  ros2-start.service
sudo systemctl start   ros2-start.service
```

### Birim (Unit) Türleri
| Tip	| Uzantı	| Ne İşe Yarar? |
|-----|---------|-----------------|
| Service	| .service	 | Arka plan hizmetlerini (daemon) tanımlar. |
| Target	| .target |	Çeşitli birimleri gruplar, run level yerine geçer. |
| Socket	| .socket |	Socket-activated servisler tanımlar. |
| Timer	    |  .timer |	Zamanlanmış görevler (cron alternatifi). |
| Mount	    |  .mount | 	Dosya sistemleri otomatik monteler. |
| Path	    | .path |	Dosya/dizin değişikliklerini tetikleyici yapar. |