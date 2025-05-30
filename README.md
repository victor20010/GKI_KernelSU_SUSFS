### Это репозиторий для автоматической сборки GKI ядра

> Для не-GKI можно попробовать ресурсы [SukiSU облачного диска](https://alist.shirkneko.top)
> При первом использовании обязательно **внимательно прочитайте** следующее содержимое, не занимайте время других из-за лени!

### Загрузка
Вы можете [скачать здесь](https://github.com/zzh20188/GKI_KernelSU_SUSFS/releases) ваши ресурсы
1. Относительно Anykernel3.zip - скачивайте и используйте!
- Затем используйте программу для прошивки, например [HorizonKernelFlasher](https://github.com/libxzr/HorizonKernelFlasher/releases) для прошивки ядра
2. Относительно boot.img - скачивайте соответствующий формату вашего ядра (без сжатия, gz, lz4), [справка](https://kernelsu.org/zh_CN/guide/installation.html#install-by-kernelsu-boot-image) **найдите подходящий boot.[...]
- Используйте [FASTBOOT](https://magiskcn.com/) для прошивки, или используйте программы прошивки для записи в boot раздел слота где находится ROOT (например, 爱玩机, Kernelflasher)

### Поддержка
| Функция | Описание |
| --- | --- |
| [KernelSU](https://kernelsu.org/zh_CN/) | Включает **оригинальную, MKSU, SUKISU, NEXT** |
| [SUSFS4](https://gitlab.com/simonpunk/susfs4ksu) | Патч функции помощи скрытия KSU на уровне ядра |
| [BBR](https://blog.thinkin.top/archives/ke-pu-bbrdao-di-shi-shi-me) | Алгоритм контроля перегрузки TCP, делает сеть быстрее? |
| [Wireguard](https://zh.wikipedia.org/wiki/WireGuard) | См. ссылку на wiki слева |
| [LZ4KD](https://github.com/ShirkNeko/SukiSU_patch/tree/main/other) | Говорят, это алгоритм ZRAM из исходников HUAWEI, патч портирован [云彩之枫](http://www.coolapk.com/u/24963680) |

<details>

<summary>Также поддерживает эти несколько алгоритмов, можно переключать в ZRAM в scene</summary>

### LZ4K, LZ4HC, deflate, 842, ~~zstdn~~, lz4k_oplus

</details>

### Менеджер KSU
После завершения компиляции вы увидите файл типа `Next-Manager(12600)`, проще говоря - это ***последний менеджер***, загруженный вместе с ядром.
![Пример](./assets/get_manager.gif)
Аналогично, в [Release](https://github.com/zzh20188/GKI_KernelSU_SUSFS/releases) также содержится ***последний менеджер***!
![release](./assets/release_manager.gif)

### Руководство по экстренному восстановлению

> [!IMPORTANT]
> **Условия срабатывания**  
> Когда устройство не может загрузиться по следующим причинам, необходимо выполнить восстановление:  
> - Прошивка неправильного/несовместимого ядра
> - Исключение совместимости версии ядра (например, прошивка ядра версии 233 на 5.10.66)

1. Войти в режим FASTBOOT

- Комбинация физических клавиш: питание + громкость- или команда ADB: `adb reboot bootloader`

2. Выполнить команду прошивки
```bash
$ fastboot flash boot <полное_имя_файла_boot.img>
```

### Пути получения оригинального образа
1. Извлечение из существующей прошивки

- Пакет для прошивки через recovery: распакуйте и используйте [инструмент payload-dumper](https://magiskcn.com/payload-dumper-go-boot.html)

- Пакет для прошивки через fastboot: напрямую распакуйте для получения boot.img

2. Получение из внешних ресурсов

- Поиск на платформах сообщества: модель + оригинальный boot (например XDA/酷安)

- [Онлайн извлечение на мобильном устройстве удаленное получение](https://magiskcn.com/payload-dumper-compose.html)

> [!TIP]
> ### Объяснение совместимости версий ядра
> 
> **1. Правила прошивки между подверсиями**  
> Когда основная версия GKI телефона 5.10.x (например 5.10.168), можно прошивать ядро с более высокой подверсией той же основной версии (например 5.10.198).  
> Относительно версии **X-lts**, на примере `android12-5.10.X-lts-AnyKernel3.zip`:
> - **X-lts** означает версию долгосрочной поддержки (максимальный номер подверсии, в текущем примере 5.10.236)
> - LTS будет продолжать увеличивать номер версии компиляции с обновлением исходного кода GKI (другие версии как 198 фиксированы навсегда)
> - ⚠️ Внимание: хотя LTS самая новая, **но** новейшая версия ≠ самая стабильная (например, 6.6.x имеет баг автоматической перезагрузки)
> 
> **2. Метод маскировки версии ядра**  
> Выполните в терминале MT менеджера:
> ```bash
> uname -r
> ```
> Получите текущий номер версии ядра, введите этот номер версии в панель компиляции Action для реализации маскировки версии ядра.
> 
> **3. Рекомендации по оптимизации компиляции**  
> Измените [файл конфигурации workflow](https://github.com/zzh20188/GKI_KernelSU_SUSFS/tree/dev/.github/workflows) (например kernel-a12-5.10.yml):
> - ▶️ Удалите/закомментируйте ненужные конфигурации версий GKI (**ускорение компиляции**)
> - ➕ Добавьте указанную версию GKI (см. [руководство по настройке](https://www.coolapk.com/feed/62820671?shareKey=OGMxYmZmNTk0YzIxNjgxNzM1MzI~&shareUid=11253396&shareFrom=com.coolapk.market_15.2.2))
> 
> **4. Известные проблемы совместимости**  
> - Некоторые версии GKI (например 5.10.66) не компилируются в среде `ubuntu-latest`
> - Ядро 6.6.x временно не поддерживает патчи KPM (есть проблемы совместимости)

### Дополнительный контент
Можете высказать ваши мнения... Я попробую!
