# Запуск Mina с Докером

## 1. Подготовка к запуску

### 1.1 Настройка Ubuntu

Обновляем пакеты на сервере до новейших версий:

```text
sudo apt update && sudo apt upgrade -y
```

Установим Докер:

```text
sudo apt install docker.io curl -y
```

Активируем Докер:

```text
sudo systemctl start docker
sudo systemctl enable docker
```

### 1.2 Настройка Фаервола

{% hint style="info" %}
Если на вашем VPS сервере есть встроенный Фаервол, то следует там открыть порты 8302 и 8303.  
  
Если же его нет, то следуйте командам ниже:
{% endhint %}

Открываем порты 8302 и 8303:

```text
sudo iptables -A INPUT -p tcp --dport 8302 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 8303 -j ACCEPT
```

## 2. Варианты запуска ноды

{% hint style="warning" %}
Выберите только один вариант запуска из 2-х предложенных ниже \(пункт 2.1 или 2.2\).
{% endhint %}

### 2.1 Запуск только Производителя блоков \(Block Producer\):

Описание изменяемых переменных:

`--name mina` - имя для контейнера можно использовать любое, либо оставить так, как есть  
`-work-selection seq` или `-work-selection rand` 

```text
sudo docker run --name mina -d \
--restart always \
-p 8301-8305:8301-8305 \
-p 127.0.0.1:3085:3085 \
-v $(pwd)/peers.txt:/root/peers.txt \
-v $(pwd)/keys:/root/keys:ro \
-v $(pwd)/.coda-config:/root/.coda-config \
minaprotocol/mina-daemon-baked:4.1-turbo-pickles-mina8245234-auto3a4e5ce daemon \
-peer-list-file $HOME/peers.txt \
-block-producer-key $KEYPATH \
-block-producer-password $CODA_PRIVKEY_PASS \
-insecure-rest-server \
-log-level Info \
-work-selection rand
```

### 2.2 Запуск Снарк Воркера \(Snark Worker\) к Производителю Блоков:

{% hint style="warning" %}
Если вы не хотите запускать Snark Worker. Вы можете сразу перейти к шагу 3.
{% endhint %}

Для начала нужно закрыть порт 3085 для входящих соединений:

```text
iptables -I INPUT 1 -p tcp --sport 3085 -j DROP
```

#### Теперь можно запускать Снарк Воркер.

Установим комиссию Воркера:  
`set-snark-work-fee 0.025` - значение комиссии `0.025` можно сменить на любое другое.

```text
sudo docker exec -it mina coda client set-snark-work-fee 0.025
```

Запустим Воркер:

```text
sudo docker exec -it mina coda client set-snark-worker -address $MINA_PUBLIC_KEY
```

{% hint style="info" %}
Для работы одновременно Производителя блоков \(Block Producer\) и Снарк Воркера \(Snark Worker\) можно настраивать Снарк Стоппер. Чтобы ненадолго останавливать Воркера во время производства блока.

Перейдите по ссылке ниже, чтобы настроить Снарк Стоппер.
{% endhint %}

{% page-ref page="../nastroika-snark-stoppera.md" %}

### 2.3 Запуск только Снарк Воркера \(без Производителя Блоков\)

Для начала запустим ноду без Производителя и Снарка.

Описание изменяемых переменных:

`--name mina` - имя для контейнера можно использовать любое, либо оставить так, как есть  
`-work-selection seq` или `-work-selection rand` 

```text
sudo docker run --name mina -d \
--restart always \
-p 8301-8305:8301-8305 \
-p 127.0.0.1:3085:3085 \
-v $(pwd)/peers.txt:/root/peers.txt \
-v $(pwd)/keys:/root/keys:ro \
-v $(pwd)/.coda-config:/root/.coda-config \
minaprotocol/mina-daemon-baked:4.1-turbo-pickles-mina8245234-auto3a4e5ce daemon \
-peer-list-file $HOME/peers.txt \
-insecure-rest-server \
-log-level Info \
-work-selection seq
```

Теперь переходим выше к пункту **2.2** и запускаем Снарк Воркера.

## 3. Просмотр логов

Посмотреть запущенные контейнеры:

```text
sudo docker ps -a
```

Логи контейнера с нодой:

```text
sudo docker logs --follow mina -f
```

Статус ноды:

```text
sudo docker exec -it mina coda client status
```

### 3.1 Альтернативный вывод логов

```text
sudo docker exec -it mina coda client status | grep "Block producers"
```

Вывод покажет только строку с запущенным производителем блоков. Пример ниже:

{% code title="\#ПРИМЕР" %}
```text
root@Mina:~# sudo docker exec mina coda client status | grep "Block producers"
Block producers running:         1 (B62qpSphT9prqYrJFio82WmV3u29DkbzGprLAM3pZQM2ZEaiiBmyY82)
```
{% endcode %}

![](../../.gitbook/assets/image.png)

## 4. Команды Докера

Остановка контейнера осуществляется командой:

```text
sudo docker stop mina
```

Удаление контейнера:

```text
sudo docker rm mina
```

Удаление запущенного контейнера:

```text
sudo docker rm -f mina
```

