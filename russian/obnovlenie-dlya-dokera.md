---
description: Простые шаги для обновления ноды.
---

# Обновление ноды для Докера

## 1. Удаление

Остановка и удаление контейнера:

```
sudo docker rm -f mina
```

## 2. Настройка

Удаление всех остановленных контейнеров и неиспользуемых образов:

```
sudo docker system prune -a
```

Скачиваем новое обновление:&#x20;

```
sudo docker pull minaprotocol/mina-daemon:1.3.1.1-f361ba1-bionic-mainnet
```

## 3. Запуск

Все готово к запуску. \
Теперь выберите нужный пункт для запуска из раздела №2 по ссылке ниже:

{% content-ref url="varianty-zapuska-nody/zapusk-coda-s-docker.md" %}
[zapusk-coda-s-docker.md](varianty-zapuska-nody/zapusk-coda-s-docker.md)
{% endcontent-ref %}

## 4. Дополнительно (не обязательно)

Если вам нужно очистить папку с конфигом запустите команду ниже:

```
rm -rf $HOME/.mina-config
```
