# Запуск Mina с Докером

## 1. Подготовка к запуску

### 1.1 Настройка Ubuntu

Обновляем пакеты на сервере до новейших версий:

```
sudo apt update && sudo apt upgrade -y
```

Установим и активируем Докер:

```
sudo apt install docker.io curl -y \
&& sudo systemctl start docker \
&& sudo systemctl enable docker
```

### 1.2 Настройка Фаервола

{% hint style="info" %}
Если на вашем VPS сервере есть встроенный Фаервол, то следует там открыть порты 8302 и 8303.\
\
Если же его нет, то следуйте командам ниже:
{% endhint %}

Открываем порты 8302 и 8303:

```
sudo iptables -A INPUT -p tcp --dport 8302:8303 -j ACCEPT
```

## 2. Варианты запуска ноды

{% hint style="warning" %}
Выберите только один вариант запуска из 2-х предложенных ниже (пункт 2.1 или 2.2).
{% endhint %}

### 2.1 Запуск только Производителя блоков (Block Producer):

Описание изменяемых переменных:

`--name mina` - имя для контейнера можно использовать любое, либо оставить так, как есть;\
`--env MINA_PRIVKEY_PASS='YOUR PASS'` - вместо `YOUR PASS` укажите пароль от вашего ключа.\
`--env UPTIME_PRIVKEY_PASS='YOUR PASS'` - вместо `YOUR PASS` укажите пароль от вашего ключа для программы делегации.\
`$KEYPATH` - путь к файлу с приватным ключем `my-wallet`. \
\
! Обязательно:\
`--coinbase-receiver B62qp...` - флаг перенаправления награды за блок на другой адрес.

```
sudo docker run --name mina -d \
--restart always \
-p 8302:8302 \
-p 127.0.0.1:3085:3085 \
-v $(pwd)/keys:/root/keys:ro \
-v $(pwd)/.mina-config:/root/.mina-config \
--env MINA_PRIVKEY_PASS='YOUR PASS' \
--env UPTIME_PRIVKEY_PASS='YOUR PASS' \
minaprotocol/mina-daemon:1.3.1.1-f361ba1-bionic-mainnet daemon \
--block-producer-key $KEYPATH \
--coinbase-receiver $YOUR_ADDRESS \
--peer-list-url https://storage.googleapis.com/mina-seed-lists/mainnet_seeds.txt \
--uptime-submitter-key $KEYPATH \
--uptime-url https://uptime-backend.minaprotocol.com/v1/submit \
--insecure-rest-server \
--open-limited-graphql-port \
--limited-graphql-port 3095 \
--file-log-level Debug \
-log-level Info
```

### 2.1.1 Запуск Снарк Воркера (Snark Worker) к Производителю Блоков:

{% hint style="warning" %}
Если вы не хотите запускать Snark Worker вместе с Производителем Блоков. Вы можете сразу перейти к шагу 3.
{% endhint %}

Установим комиссию Воркера:\
`set-snark-work-fee 0.025` - значение комиссии `0.025` можно сменить на любое другое.

```
sudo docker exec -it mina mina client set-snark-work-fee 0.025
```

Запустим Воркер:

```
sudo docker exec -it mina mina client set-snark-worker -address $MINA_PUBLIC_KEY
```

{% hint style="info" %}
Для работы одновременно Производителя блоков (Block Producer) и Снарк Воркера (Snark Worker) можно настраивать Снарк Стоппер. Чтобы ненадолго останавливать Воркера во время производства блока.

Перейдите по ссылке ниже, чтобы настроить Снарк Стоппер.
{% endhint %}

{% content-ref url="../nastroika-snark-stoppera.md" %}
[nastroika-snark-stoppera.md](../nastroika-snark-stoppera.md)
{% endcontent-ref %}

### 2.2 Запуск только Снарк Воркера (без Производителя Блоков)

Описание изменяемых переменных:

`--name mina` - имя для контейнера можно использовать любое, либо оставить так, как есть;

По умолчанию `-work-selection` для Снарк Воркера является случайным `rand`.\
Вы можете изменить это, добавив флаг `-work-selection seq` в конец команды запуска, которая будет работать с заданиями в том порядке, в котором они должны быть включены из состояния сканирования и скорее всего приведет к включению ваших снарков без потенциально длительной задержки;

`set-snark-work-fee 0.025` - значение комиссии Воркера `0.025` можно сменить на любое другое.

```
sudo docker run --name mina -d \
--restart always \
-p 8302:8302 \
-p 127.0.0.1:3085:3085 \
-v $(pwd)/keys:/root/keys:ro \
-v $(pwd)/.mina-config:/root/.mina-config \
minaprotocol/mina-daemon:1.3.1.1-f361ba1-bionic-mainnet daemon \
--peer-list-url https://storage.googleapis.com/mina-seed-lists/mainnet_seeds.txt \
--snark-worker-fee 0.025 \
--run-snark-worker $MINA_PUBLIC_KEY \
--insecure-rest-server \
--file-log-level Debug \
-log-level Info
```

## 3. Просмотр логов

Посмотреть запущенные контейнеры:

```
sudo docker ps -a
```

Логи контейнера с нодой:

```
sudo docker logs --follow mina -f --tail 1000
```

Статус ноды:

```
sudo docker exec -it mina mina client status
```

### 3.1 Альтернативный вывод логов

```
sudo docker exec -it mina mina client status | grep "Block producers"
```

Вывод покажет только строку с запущенным производителем блоков. Пример ниже:

{% code title="#ПРИМЕР" %}
```
root@Mina:~# sudo docker exec -it mina mina client status | grep "Block producers"
Block producers running:         1 (B62qpSphT9prqYrJFio82WmV3u29DkbzGprLAM3pZQM2ZEaiiBmyY82)
```
{% endcode %}

![](../../.gitbook/assets/image.png)

## 4. Команды Докера

Остановка контейнера осуществляется командой:

```
sudo docker stop mina
```

Рестарт контейнера

```
sudo docker restart mina
```

Удаление контейнера:

```
sudo docker rm mina
```

Удаление запущенного контейнера:

```
sudo docker rm -f mina
```

## 5. Разное

Удаление папки с конфигом:

```
rm -rf $HOME/.mina-config
```
