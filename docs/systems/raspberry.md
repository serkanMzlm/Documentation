# Raspberry Pi

- GPIO numaralarının karşılıklarını görmek için `/sys/kernel/debug/gpio` dosyasına bakılabilir

```bash
sudo cat /sys/kernel/debug/gpio
```

- `/etc/udev/rules.d` dosyasının altına belirli kurallar eklenebilir bu kısımda eklenen dosyanın başında yazan sayı öncelik durumunu belirtir ne kadar büyük bir sayıyısa o kadar sonra olucagını ayarlar. 

### GPIO Erişilebilir Yapmak (rules)

```bash
sudo nano /etc/udev/rules.d/99-gpio-permissions.rules
```
```
SUBSYSTEM=="gpio*", PROGRAM="/bin/sh -c 'chown -R root:gpio /sys/class/gpio && chmod -R 770 /sys/clas>
```

```bash
sudo udevadm control --reload-rules
```