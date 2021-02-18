---
description: Простые шаги для обновления ноды в Сервисе.
---

# Обновление для Сервиса

## 1. Остановка Сервиса

Остановка:

```text
systemctl --user stop mina
```

## 2. Настройка

Удаление предыдущего обновления:

```text
sudo apt-get remove mina-testnet-postake-medium-curves
```

Скачиваем новое обновление: 

```text
echo "deb [trusted=yes] http://packages.o1test.net release main" | sudo tee /etc/apt/sources.list.d/mina.list
sudo apt-get update
sudo apt-get install -y curl unzip mina-testnet-postake-medium-curves=0.4.2-245a3f7
```

Где `0.4.2` - версия нового пакета.

Скачивание новых пиров:

```text
wget -O ~/peers.txt https://storage.googleapis.com/seed-lists/zenith_seeds.txt
```

## 3. Изменение конфигурации

{% hint style="info" %}
Делайте этот пункт только если нужно изменить что-либо в конфигурационных файлах `.mina-env` или файл сервиса`mina.service .`

Например в последнем обновлении нужно добавить флаг `-super-catchup`
{% endhint %}

Заходим в файл с флагами:

```text
nano .mina-env
```

Добавляем флаг `-super-catchup` в конец строки `EXTRA_FLAGS`

```text
EXTRA_FLAGS=" -file-log-level Debug"
```

Сохраняем и выходим: CTRL+S и CTRL+X

## 4. Запуск

Перезагружаем изменение конфигурации:

```text
systemctl --user daemon-reload
```

Запускаем Сервис:

```text
systemctl --user restart mina
```

Просмотр логов:

```text
journalctl --user-unit mina -n 1000 -f
```

