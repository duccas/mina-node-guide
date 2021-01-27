---
description: Простые шаги для обновления ноды.
---

# Обновление для Докера

## 1. Удаление

Остановка и удаление контейнера:

```text
sudo docker rm -f mina
```

## 2. Настройка

Удаление всех остановленных контейнеров и неиспользуемых образов:

```text
sudo docker system prune -a
```

Скачиваем новое обновление: 

```text
sudo docker pull minaprotocol/mina-daemon-baked:0.2.10-62e11d5-testworld-62e11d5
```

Где `minaprotocol/mina-daemon-baked:0.2.10-62e11d5-testworld-62e11d5` - версия нового образа докера.

## 3. Запуск

Все готово к запуску.   
Теперь выберите нужный пункт для запуска из раздела №2 по ссылке ниже:

{% page-ref page="varianty-zapuska-nody/zapusk-coda-s-docker.md" %}

## 4. Дополнительно \(не обязательно\)

Если вам нужно очистить папку с конфигом запустите команду ниже:

```text
rm -rf $HOME/.coda-config
```

