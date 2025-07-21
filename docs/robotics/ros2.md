# ROS2 
- **ROS (Robot Operating System):** Robot uygulamaları geliştirmek için sunduğu mesajlaşma altyapısı ve araç setiyle, farklı yazılım bileşenlerinin (düğüm, node) birbirleriyle kolayca iletişim kurmasını sağlar.

| Özellik |	ROS 1	| ROS 2 |
|---------|---------|-------|
| Mimari |	Merkezi (master node) |	Merkeziyetsiz (no master) |
| Gerçek Zaman Desteği	| Sınırlı	| DDS tabanlı, gerçek zaman yetenekli |
| Çoklu Dil Desteği| C++, Python	| C++, Python, Java, Rust, … |
| Ağ Desteği |	Aynı ağda (multicast)| 	Farklı ağlarda firewall dostu |
| Güvenlik	| Ek eklentiler gerekir |	DDS Security eklentisi |

- Yeni bir paket oluşturduğunuzda, `src/` altında dil fark etmeksizin (C++, Python vb.) `ros2 pkg create` ile paket iskeletleri oluşturulur.
- Bir ROS/ROS 2 çalışma alanı (workspace), genellikle şu dizinleri içerir:

```
<workspace>/
├─ src/        # Paket kaynak kodları
├─ build/      # Derleme dosyaları
├─ install/    # Kurulum (derlenmiş paketler)
└─ log/        # Derleyici ve runtime logları
```

- **RCL:** “ROS Client Library”
- **RCLCPP / RCLPY:** C++ ve Python için üst katman kütüphaneleri.
- Mesajlaşma, parametre, zamanlayıcı, servis ve aksiyon gibi temel API’leri sunar.

- **RQT:** Qt tabanlı GUI panelleri; topic’ler, parametreler, servislere erişim ve grafik çizimler yapar.
- **ros2 doctor --report:** Sistem yapılandırmanızı analiz eder, eksik bağımlılıkları ve uyumsuzlukları raporlar.

```bash
NETWORK CONFIGURATION

PLATFORM INFORMATION

RMW MIDDLEWARE

ROS 2 INFORMATION

TOPIC LIST
```

### Logging (Günlükleme)

- **Log seviyeleri:** `DEBUG, INFO, WARN, ERROR, FATAL`.
```c++
RCLCPP_INFO(node->get_logger(), "Bilgi mesajı");
RCLCPP_WARN(node->get_logger(), "Uyarı mesajı");
RCLCPP_ERROR(node->get_logger(), "Hata mesajı");
RCLCPP_DEBUG_STREAM(node->get_logger(), "Detay: " << variable);
```

```bash title="Komut Satırları"
ros2 run <pkg> <exe> --ros-args --log-level debug
ros2 run <pkg> <exe> --ros-args --log-level <node_name>:=warn
```

- Log dosyaları `~/.ros/log/` altında saklanır `rqt_logging` ile de izleyebilirsiniz.

### DDS ve DOMAIN ID
- ROS 2, **DDS** (Data Distribution Service) protokolünü kullanır.
- **Domain ID:** Aynı DDS domain’inde olan düğümler birbirini görür.
    - Varsayılan: `0`
    - Güvenli aralık: `0–101` (Linux port çakışmalarından kaçınmak için).
    - DD S UDP port aralığı: `0–65535` → Domain ID `0–232` önerilir.
```bash
export ROS_DOMAIN_ID=42
# Geçerli aralığı kontrol
cat /proc/sys/net/ipv4/ip_local_port_range
```

- `export ROS_LOCALHOST_ONLY=1` Düğümler yalnızca localhost üzerinden iletişim kurar; diğer cihazlardan erişim engellenir.

### Derleme Zamanında Hata Ayıklama
- `colcon build --cmake-args -DCMAKE_BUILD_TYPE=RelWithDebInfo` 
    - **RelWithDebInfo:** Optimize edilmiş kodda hata ayıklama sembollerini tutar.
- `ros2 run --prefix "gdbserver localhost:3000" <pkg_name> <node_executable>`
    - **gdbserver:** Kodunuzu dinleyen bir sunucu gibi çalışır.
    - **localhost:3000:** VS Code veya başka bir istemci bu port üzerinden hata ayıklamayı üstlenir.

- vs code launch.json  dosyası:
```json title="launch.json" linenums="1" hl_lines="10"
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "ROS2 C++ Remote Debug",
      "type": "cppdbg",
      "request": "launch",
      "miDebuggerServerAddress": "localhost:3000",
      "miDebuggerPath": "/usr/bin/gdb",
      "program": "${workspaceFolder}/install/<pkg_name>/lib/<pkg_name>/<node_executable>",
      "cwd": "${workspaceFolder}",
      "stopAtEntry": false,
      "environment": [],
      "externalConsole": false
    }
  ]
} 
```

- **program:** Hata ayıklamak istediğiniz executable’ın tam yolu (colcon install sonrası).
- **miDebuggerServerAddress:** gdbserver’ın dinlediği adres ve port.
- Diğer ayarlar (stopAtEntry, environment) tercihinize göre düzenlenebilir.

- VS Code’da `Run & Debug` panelinden bu konfigürasyonu seçip başlatın. Kod, gdbserver üzerinden bağlanacak ve breakpoint’lerinizde duracaktır.

- Menüden Terminal → Run Build Task ya da `Ctrl+Shift+P`
```json title="tasks.json" linenums="1" 
{
  "version": "2.0.0",
  "tasks": [
    {
      "label": "Build Release",
      "type": "shell",
      "command": "source /opt/ros/${command:rosdistro}/setup.bash && colcon build --symlink-install"
    },
    {
      "label": "Build Debug",
      "type": "shell",
      "command": "source /opt/ros/${command:rosdistro}/setup.bash && colcon build --cmake-args -DCMAKE_BUILD_TYPE=RelWithDebInfo --symlink-install"
    }
  ]
} 
```

###  DDS Katmanını Seçme

```bash
export RMW_IMPLEMENTATION=rmw_cyclonedds_cpp
# veya
export RMW_IMPLEMENTATION=rmw_fastrtps_cpp
```
