# CMake Kullanım

CMake, yazılım projelerinin derleme sürecini yöneten, farklı derleyiciler ve işletim sistemleri arasında uyumluluk sağlayan güçlü bir araçtır. Projelerin derleme seçeneklerini, bağımlılıklarını tanımlayarak yapılandırma işlemlerini otomatikleştirir.

## Temel Komutlar

```cmake 
cmake --help                     # CMake ve build yöntemleri hakkında genel bilgi.
cmake --help-variable-list       # Kullanılabilir değişkenleri listeler.
cmake --help-variable [variable] # Belirtilen değişken hakkında detaylı bilgi.

# Genel kullanım:
cmake -G "Ninja" -DCMAKE_BUILD_TYPE=Release -S . -B build
cmake -P CMakeLists.txt  # Build işlemi yapmadan CMake dosyasını çalıştırır.
```

## Projeyi Derleme Yöntemleri

=== "Yöntem 1"

    ```cmake
    mkdir build  
    cd build  
    cmake ..  
    make
    ```

=== "Yöntem 2"

    ```cmake
    cmake -S . -B build  
    cd build  
    make 
    ```

=== "Yöntem 3"

    ```cmake
    cmake -B build  
    cmake --build build
    ```

## CMake Dosya Yapısı

- CMakeLists.txt ana dizinlerde kullanılır.

- `.cmake` uzantılı dosyalar script veya modül olarak kullanılır:
    - Directories (CMakeLists.txt)
    - Scripts (`<script>.cmake`)
    - Modules (`<module>.cmake`)
- `if - elseif - else`: Mantıksal işlemler.
- `foreach` ve `while` döngüleri: Yinelemeli işlemler için.
- `function`: Lokal kapsam.
- `macro`: Global kapsam.

## Değişkenler

| Değişken | Anlamı |
|--------|--------|
| `PROJECT_NAME`              | project() komutunda belirtilen proje adı |
| `CMAKE_PROJECT_NAME`        | En üst dizindeki project() adı |
| `CMAKE_VERSION`             | CMake sürümü (örn: 3.16.1) |
| `CMAKE_GENERATOR`           | Yapı oluşturucuyu belirtir (Ninja, Unix Makefiles) |
| `CMAKE_SOURCE_DIR`          | Ana proje dizini |
| `CMAKE_CURRENT_SOURCE_DIR`  | Bulunulan dizin |
| `PROJECT_SOURCE_DIR`        | En son çağrılan project() dizini |
| `CMAKE_BINARY_DIR`          | Build dizini |
| `CMAKE_HOME_DIRECTORY`      | En üst dizin yolu |
| `CMAKE_SYSTEM`              | İşletim sisteminin tam adı |
| `CMAKE_SYSTEM_NAME`         | İşletim sistemi adı (Linux, Windows) |
| `CMAKE_INSTALL_PREFIX`      | Kurulum dizini |
| `CMAKE_MODULE_PATH`         | Ek CMake modüllerinin yolu |

## Temel Komutlar ve Kullanımları

- `cmake_minimum_required`: Kullanılması gereken minimum CMake sürümünü belirtir.
- `project`: Projeyi isimlendirir, kullanılacak dilleri ve versiyonları belirtir.
- `add_executable`: Yürütülebilir dosya oluşturur.
- `add_library`: Kütüphane hedefleri oluşturur.
- `add_subdirectory`: Alt dizindeki CMake dosyasını çalıştırır.
- `target_include_directories`: Hedefe özel header dizinlerini tanımlar.
    - **PUBLIC:** Hem hedef hem tüketiciler
    - **PRIVATE:** Sadece hedef
    - **INTERFACE:** Sadece tüketiciler
- `install`: Dosyaların belirli dizinlere kurulmasını sağlar
- `find_package`: CMake modüllerini bulur ve kullanıma hazırlar.
- `add_definitions`: Derleme zamanı tanımlar ekler.
- `option`: Derleme seçenekleri sunar.
- `add_compile_options`: Derleyici parametreleri ekler.
- `file(GLOB)` ve `file(GLOB_RECURSE)`: Dosya listeleri oluşturur.
- `execute_process`: Terminal komutlarını çalıştırır.
- `set_property`: Nesnelere özellik atar.
- `set_target_properties`: Hedef özelliklerini belirler.
- `cmake_policy`: Uyumluluk davranışlarını yönetir.
- `add_custom_command` ve add_custom_target: Özel build komutları oluşturur.

```cmake
cmake_minimum_required(VERSION 3.10)
project(myProject LANGUAGES C CXX VERSION 1.0)

find_package(my_log CONFIG REQUIRED)

option(ENABLE_FEATURE "Enable feature X" OFF)

file(GLOB_RECURSE SRC_FILES ${PROJECT_SOURCE_DIR}/src/*.cpp)
execute_process(COMMAND git pull WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR})

include_directories(${PROJECT_SOURCE_DIR}/inc)
add_library(my_add add.cpp)
add_executable(my_project main.cpp)
target_link_libraries(my_project PRIVATE my_add)
add_subdirectory(add_dir)

set_target_properties(myTarget PROPERTIES COMPILE_FLAGS "-O3 -Wall")

target_include_directories(my_add PUBLIC ${PROJECT_SOURCE_DIR}/inc)

install(FILES include/log.hpp DESTINATION include)
install(TARGETS myLib EXPORT my_export DESTINATION lib)
```

!!! note "Not"
    Cache Değişkenleri, belleğe alınan değişkenler, tekrar hesaplanmayı önler ve hızlı erişim sağlar.
    ```cmake
    set(VAR "12" CACHE STRING "Açıklama")
    ```

Bu rehber, CMake'in temel ve gelişmiş özelliklerini tanıtmakta olup, projelerinizi etkili şekilde yönetmeniz için gerekli bilgileri sunar.