# Kconfig ve Menuconfig Kullanım

Kconfig/Menuconfig, yazılım projelerinde kullanıcı tarafından ayarlanabilir yapılandırma seçenekleri oluşturmak için kullanılan bir sistemdir. Linux çekirdeği başta olmak üzere birçok projede yaygın olarak kullanılır.

## Temel Komutlar ve Yapılar

```kconfig
mainmenu "Ana Menü Başlığı"

comment "Bilgi veya Açıklama: $(VARIABLE)"

menu "Alt Menü Başlığı"
    config CONFIG_NAME
        bool "Görünecek İsim"
        default y
        help
            Bu ayarın ne işe yaradığını açıklar.
endmenu
```

## Temel Elemanlar

- `mainmenu`: Ana başlığı belirler.
- `comment`: Bilgilendirme amacıyla kullanılır.
- `menu / endmenu`: Alt menü oluşturur.
- `choice / endchoice`: Kullanıcıya seçim grubu oluşturur.

## Değişken Türleri (config)

- `bool`: Açık veya kapalı (y/n)
- `tristate`: Üç seçenekli; kapalı (n), açık (y) veya modül (m)
- `string`: Metin verisi
- `int`: Tam sayı
- `hex`: Hexadecimal sayı

## Örnek Yapılar

```kconfig
menu "Toolchain"

    choice
        prompt "Platform"
        default PLATFORM_NUTTX

        config PLATFORM_NUTTX
            bool "nuttx"

        config PLATFORM_POSIX
            bool "posix"
    endchoice

    config USER_NAME
        string "Username"
        default "serkan"
        help
            Enter your username.

    config USER_AGE
        int "User Age"
        default 5
        range 1 8
        help
            Enter your age (1-8).

    config BOARD_PLATFORM
        string
        default "nuttx" if PLATFORM_NUTTX
        default "posix" if PLATFORM_POSIX

    config CLOCK_PLL_N
        int "PLL Clock"
        depends on USE_CLOCK_PLL
        default 20
        range 8 86

endmenu
```

## Şartlı Menüler ve Seçimler

Menülerin ve seçeneklerin görünürlüğü koşullara bağlı olabilir.

```kconfig
menu "Drivers"
    depends on PLATFORM_QURT || PLATFORM_POSIX
    source "src/Kconfig"
endmenu
```

## Seçime Bağlı Otomatik Aktifleştirme (select)

Bir yapılandırma seçeneği seçildiğinde başka seçenekleri otomatik olarak etkinleştirmek için kullanılır.
```kconfig
config MODULE_PERIPH_ADC
    bool "ADC peripheral driver"
    depends on PLATFORM_NUTTX
    select MODULE_PERIPH_COMMON
```

- `tristate` tipinde yapılandırmalar üç duruma sahiptir:
    - **n:** Kapatılmış (seçim yok)
    - **y:** Sabit olarak açık
    - **m:** Modül olarak derlenir, çalışma zamanında yüklenebilir veya kaldırılabilir.

```kconfig
menu "Advanced Features"

    choice
        tristate "Enable advanced features"

        config ADV_FEATURE_1
            tristate "Feature 1"

        config ADV_FEATURE_2
            tristate "Feature 2"

        if ADV_FEATURE_2
            choice
                tristate "Sub Features"

                config SUB_FEATURE_1
                    tristate "Sub Feature 1"

                config SUB_FEATURE_2
                    tristate "Sub Feature 2"
            endchoice
        endif
    endchoice
endmenu
```

## Komutlar Hakkında Kısa Bilgiler

| Komut | Anlamı |
|--------|--------|
| `mainmenu`         | Menü ana başlığını oluşturur.|
| `menu/endmenu`     | Menü oluşturur ve sonlandırır.|
| `choice/endchoice` | Seçim grupları oluşturur.|
| `config`           | Yapılandırma seçeneklerini tanımlar.
| `default`          | Varsayılan değeri belirler.|
| `depends on`       | Şarta bağlı görünürlük sağlar.|
| `select`           | Seçenekleri otomatik etkinleştirir.|
| `range`            | Sayısal değer aralığını sınırlar.|
| `help`             | Kullanıcıya açıklama sunar.|

## Kullanım Tavsiyeleri

- Yapılandırma seçeneklerini anlamlı ve açıklayıcı isimlerle belirtin.
- `help` kısımlarını açıklayıcı yazın.
- Menüleri mantıklı ve kullanıcı dostu bir hiyerarşide tutun.
- Şarta bağlı görünebilirlik ile karmaşıklığı azaltın.

Bu rehber, Kconfig ve Menuconfig dosyalarını etkili kullanmanız için temel ve ileri düzey bilgileri içerir.
