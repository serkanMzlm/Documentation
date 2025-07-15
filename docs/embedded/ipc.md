# Inter-Process Communication (IPC)

Farklı süreçler (process) arasında veri, bilgi ve komut alışverişi yapmayı sağlayan mekanizmalardır. İşletim sisteminde paralel veya dağıtık çalışan uygulamaların birbirleriyle etkileşim kurmasını mümkün kılar.

## Uygulama Türleri
- **Multi-Process Application:** Birden fazla sürecin aynı anda bağımsız biçimde çalışıp birbirleriyle etkileşimde bulunduğu yazılımlardır.

- **Parallel Programming:** Karmaşık problemleri daha küçük alt görevlere bölüp birden fazla işlemci veya çekirdekte eş zamanlı çalıştırarak performansı artırma tekniğidir.

- **Client‑Server Applications:** İstemciler (client) sunucuya (server) istek gönderir, sunucu bu istekleri işler ve yanıt döner. Dağıtık iş yükü paylaşımı sağlar.

### 1. Pipes
- Tek yönlü (unidirectional) veri kanalıdır; bir ucu yazma, diğer ucu okuma içindir.
- Unnamed Pipes
    - Ebeveyn ve alt süreç (child) arasında geçici, adsız kanal oluşturur.
    - Genellikle `pipe()` sistem çağrısıyla yaratılır.
- Named Pipes (FIFOs)
    - Dosya sisteminde özel dosya (`/tmp/mypipe`) olarak var olur.
    - Farklı süreçler ve oturumlar arasında kullanılabilir.
    - mkfifo `/tmp/mypipe` ile oluşturulur.

### 2. Sockets
- Hem aynı makine içi hem de ağ tabanlı iki yönlü (bidirectional) iletişim sağlar.
    - **Unix Domain Sockets:** Aynı sistemde süreçler arası düşük gecikmeli iletişim.
    - **TCP/IP Sockets:** Farklı makineler arasında güvenilir bağlantı.
    - **UDP Sockets**: Bağlantısız, düşük gecikmeli iletişim.

```c
// Tipik TCP server pseudo-kodu
int sock = socket(AF_INET, SOCK_STREAM, 0);
bind(sock, ...);
listen(sock, 5);
int client = accept(sock, ...);
read(client, buf, len);
write(client, response, rlen);
```

### 3. Message Queues
- Mesajları FIFO kuyruğu içinde saklar; süreçler arasında sıralı mesajlaşma sağlar.
    - Adlandırılmış (POSIX): mq_open, mq_send, mq_receive.
    - Unamed (System V): msgget, msgsnd, msgrcv.
    - Mesaj boyutu ve izinleri tanımlanabilir; bir kuyruğu birden çok süreç kullanabilir.

### 4. Shared Memory
- Birden çok sürecin aynı fiziksel bellek bölgesine erişmesini sağlar; en hızlı IPC yöntemlerinden biridir.
    - POSIX IPC: shm_open, mmap, shm_unlink.
    - System V IPC: shmget, shmat, shmdt, shmctl.

!!! note "Dikkat"
    Mutlak senkronizasyon (mutex, semafor) kullanılmalı, yoksa yarış durumları (race condition) oluşur.

### 5. Semaphores
- Bir veya daha fazla sürecin paylaşılan kaynaklara erişimini senkronize etmek için kullanılır.
    - **POSIX:** sem_open, sem_wait, sem_post, sem_close
    - **System V:** semget, semop, semctl.
    - Shared memory ile birlikte sıkça tercih edilir.

### 6. Signals
- Süreçlere asenkron bildirim (interrupt) göndermek için kullanılır.
- Standart sinyaller: `kill(pid, SIGTERM);`
    - **SIGINT**  → Ctrl+C
    - **SIGTERM** → Nazik sonlandırma
    - **SIGKILL** → Zorla sonlandırma
- Bir süreç, signal(SIGINT, handler) ile sinyal alındığında çalıştırılacak fonksiyonu tanımlayabilir.

!!! note "Not"
    Bu IPC mekanizmaları, gereksinimlerinize göre performans, güvenilirlik ve karmaşıklık açısından farklı avantajlar sunar. En hızlısı shared memory, en yaygın kullanılanı ise sockets’tir; pipes ve message queue’lar ise basit veri akışlarında idealdir.
