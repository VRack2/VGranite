![Логотип](./logo.png)

# VGgranite

Простое приложение для создания туннелей Socket -> Serial. Приложение имеет веб-интерфейс управления.

Основные возможности:

-   Добавление Socket-серверов
-   Добавление Socket-клиентов
-   Добавление последовательных портов (COM-портов)
-   Создание туннелей между портами

Приложение отслеживает возможность создания сервера или открытия последовательного порта. Если, например, преобразователь USB-to-Serial был переподключен, приложение автоматически заново открывает последовательный порт.

**Требования:** nodejs > 16v или bun последней версии

## ПОСЛЕДНЕЕ ОБНОВЛЕНИЕ 2.0.0
 - Переход на VRackDB ^3.0.2
 - Переход на экосистему vrack-core ^1.0.0
 - Работа в качестве сервиса vrack2-service

## УСТАНОВКА

### Установка (linux/ubuntu) как сервис vrack2-service

1. Склонируйте репозиторий в `/opt/vrack2-service` если вы еще не устанавливали VRack2:  
    ```bash
    git clone https://github.com/VRack2/vrack2-service.git /opt/vrack2-service
    cd /opt/vrack2-service
    ```
2. Установите зависимости:
    ```bash
    npm install
    ```
3. Создайте директории:
    ```bash
    mkdir -p devices storage structure ./devices/vgranite
    ```
4. Cкачайте [последний релиз](https://github.com/VRack2/VGranite/releases)
5. Распакуйте содержимое архива в `/opt/vrack2-service/devices/vgranite`
6. Установите зависимости VGranite:
    ```bash
    cd /opt/vrack2-service/devices/vgranite
    npm install
    ```
7. Запуск VGranite
    ```bash
    cd /opt/vrack2-service
    npm run start  "./devices/vgranite/vgranite.json"
    ```

По умолчанию используются два WEB-сервера:

-   **http://хост:4000** - Веб-интерфейс - данные для входа: Логин:`admin` Пароль:`admin`
-   **http://хост:3999** - Документация API

Структуру службы и все настройки можно посмотреть в файле `./devices/vgranite/vgranite.json`. Там параметры, которые нужно изменить для смены портов и базовых настроек.

#### Автозапуск

Для автозапуска в ubuntu есть файл службы. Скопируйте его:

```bash
cp /opt/vrack2-service/devices/vgranite/vgranite.service /etc/systemd/system/
```

Обновление демона служб:

```bash
systemctl daemon-reload
systemctl enable vgranite
systemctl start vgranite
```

## РЕШЕНИЕ ПРОБЛЕМ

1. **Permission denied: '/dev/ttyUSB0'** - Возможно, попробуйте использовать команду `sudo chmod a+rw /dev/ttyUSB0`
2. Иногда при добавлении соединения происходит ошибка и разлогинивание, но потом все работает хорошо. Пока не смог исправить причину, из-за того что не могу ее воспроизвести.


<!-- СБОРКА
======

Для сборки нам нужно клонировать репозиторий:

```bash
cd /opt/
git clone https://github.com/ponikrf/VGranite.git
```

Установка зависимостей:

```bash
npm install
```

Основное приложение будет готово, но у него не будет документации и веб-интерфейса.

Чтобы собрать интерфейс, посмотрите репозиторий [VGranite-Web](https://github.com/ponikrf/VGranite-Web)

Сборка ApiDocs:

```bash
npm install apidoc -g
```

```bash
cd /opt/vgranite/
apidoc apidoc -i devices -o apidoc
```
``` -->