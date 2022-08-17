---
description: Простые шаги для обновления ноды в Сервисе.
---

# Обновление ноды для Сервиса

## 1. Остановка Сервиса

Остановка:

```
systemctl --user stop mina
```

## 2. Настройка

Скачиваем новое обновление:&#x20;

```
echo "deb [trusted=yes] http://packages.o1test.net $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/mina.list
sudo apt-get update
sudo apt-get install -y curl unzip mina-mainnet=1.3.0-9b0369c
```

## 3. Изменение конфигурации

{% hint style="info" %}
Делайте этот пункт только если нужно изменить что-либо в конфигурационных файлах `.mina-env` или файл сервиса`mina.service .`
{% endhint %}

Заходим в файл с флагами:

```
nano .mina-env
```

Изменяем данные, сохраняем и выходим: CTRL+S и CTRL+X

## 4. Запуск

Перезагружаем изменение конфигурации:

```
systemctl --user daemon-reload
```

Запускаем Сервис:

```
systemctl --user restart mina
```

Просмотр логов:

```
journalctl --user-unit mina -n 1000 -f
```
