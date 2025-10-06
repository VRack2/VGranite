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

Поддерживаемые ОС:

-   Windows
-   Mac OS
-   Linux
-   Android (termux)

## ПОСЛЕДНЕЕ ОБНОВЛЕНИЕ 2.0.0
 - Переход на VRackDB ^3.0.2
 - Переход на экосистему vrack-core ^1.0.0
 - Работа как самодостаточный продукт, так и в качестве сервиса vrack2-service

## УСТАНОВКА

### Установка как независимое приложение

1. Сначала скачайте [последний релиз](https://github.com/VRack2/VGranite/releases)

2. Распакуйте его, желательно в `/opt/vgranite`.

3. Установите зависимости: 

```bash
cd /opt/vgranite
npm install --only=production
```

**К сожалению**, я не могу положить все зависимости `npm` в `node_modules` вместе с релизом. Эта особенность связана с пакетом `serialport`.

1. Запуск:

```bash
npm run start
```


#### Автозапуск


Для автозапуска в ubuntu есть файл службы. Скопируйте его:

```bash
cp /opt/vgranite/devices/vgranite/vgranite.service /etc/systemd/system/
```


Обновление демона служб:

```bash
systemctl daemon-reload
systemctl enable vgranite
systemctl start vgranite
```

Естественно, на свой страх и риск, так как служба запускается из-под root.

### Установка как сервис vrack2

Теперь VGranite можно поставить как сервис VRack2. Это позволяет переиспользовать часть кода, что упрощает поддержку, обновление и тп.

1. Склонируйте репозиторий в `/opt/vrack2-service` если вы еще не устанавливали VRack2:  
```bash
git clone https://github.com/VRack2/vrack2-service.git /opt/vrack2-service
cd /opt/vrack2-service
```

1. Установите зависимости:
```bash
npm install
```

1. Создайте директории:
```bash
mkdir -p devices storage structure
```

1. Склонируйте репозиторий VGranite в директорию `devices`: 
```bash
git clone https://github.com/VRack2/VGranite.git /opt/vrack2-service/devices/vgranite
```

1. Установите зависимости VGranite:
```bash
cd /opt/vrack2-service/devices/vgranite
npm install
```

1. Запуск VGranite
```bash
cd /opt/vrack2-service
npm run start  "./devices/vgranite/vgranite.json"
```

По умолчанию используются два WEB-сервера:

-   **http://хост:4000** - Веб-интерфейс - данные для входа: Логин:`admin` Пароль:`admin`
-   **http://хост:3999** - Документация API

Структуру службы и все настройки можно посмотреть в файле `./devices/vgranite/vgranite.json`. Там всё интуитивно понятно, параметры, которые нужно изменить для смены портов и базовых настроек.

РЕШЕНИЕ ПРОБЛЕМ
================

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