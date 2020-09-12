# Запуск Coda с Докером

## 1. Варианты запуска ноды

{% hint style="warning" %}
ВАЖНО! Внимательно прочитайте каждый пункт гайда, чтобы не было проблем с запуском.
{% endhint %}

### Запуск Производителя блоков \(Block Producer\):

Описание изменяемых переменных:

`--name coda` - имя для контейнера можно использовать любое, либо оставить так, как есть

Данные, которые нужно будет добавить:

`-e "CODA_PRIVKEY_PASS=YOUR-PASSWORD"` - вместо `YOUR-PASSWORD` введите ваш пароль от публичного ключа

#### Нужно внести все данные в команду ниже:

```text
sudo docker run -d \
-e "CODA_PRIVKEY_PASS=YOUR-PASSWORD" \
--mount type=bind,source="$(pwd)"/keys,target=/root/keys \
--name coda \
-p 8302:8302 \
-p 8303:8303 \
--restart always \
codaprotocol/coda-daemon:0.0.12-beta-feature-bump-genesis-timestamp-3e9b174 daemon \
-block-producer-key $HOME/keys/my-wallet \
-peer $SEED1 \
-peer $SEED2
```

Пример, как должна выглядеть команда:

{% code title="\#ПРИМЕР" %}
```text
sudo docker run -d \
-e "CODA_PRIVKEY_PASS=qwerty123" \
--mount type=bind,source="$(pwd)"/keys,target=/root/keys \
--name coda \
-p 8302:8302 \
-p 8303:8303 \
--restart always \
codaprotocol/coda-daemon:0.0.12-beta-feature-bump-genesis-timestamp-3e9b174 daemon \
-block-producer-key $HOME/keys/my-wallet \
-peer $SEED1 \
-peer $SEED2
```
{% endcode %}

### Запуск Снарк Воркера \(Snark Worker\):

{% hint style="warning" %}
Если вы не хотите запускать Снарк Воркера. Можно сразу переходить к пункту 3.
{% endhint %}

Перед запуском Воркера нужно настроить Фаервол:

```text
sudo ufw allow 8305
```

Описание изменяемых переменных:

1. `--name coda-worker` - имя для контейнера можно использовать любое, либо оставить так, как есть
2. `--memory 16g` - ограничение количества оперативной памяти, которое может использовать контейнер
3. `--cpus 8` - ограничение количества ядер процессора, которые может использовать контейнер
4. `-snark-worker-fee 0.25` - можно установить комиссию Снарк Воркера

Данные, которые нужно будет добавить:

1. `-run-snark-worker <PUBLIC_KEY>` - вместо `<PUBLIC_KEY>` нужно вставить ваш публичный ключ, который выглядит так`B62qpSphT9prqYrJFio82WmV3u29DkbzGprLAM3pZQM2ZEaiiBmyY82`
2. `-peer <SEED_1>` `-peer <SEED_2>` - вместо `<SEED_1>` и `<SEED_2>` нужно будет ввести пиры, к которым подключится нода. Их перед запуском пришлют на вашу почту. Выглядеть будет примерно так: `-peer /ip4/34.74.183.100/tcp/10001/ipfs/12D3KooWAFFq2yEQFFzhU5dt64AWqawRuomG9hL8rSmm5vxhAsgr -peer /ip4/35.231.128.243/tcp/10001/ipfs/12D3KooWB79AmjiywL1kMGeKHizFNQE9naThM2ooHgwFcUzt6Yt1`

#### Нужно внести все данные в команду ниже:

```text
sudo docker run -d \
--name coda-worker \
-p 8305:8305 \
--memory 16g \
--cpus 8 \
--restart always \
codaprotocol/coda-daemon:0.0.12-beta-feature-bump-genesis-timestamp-3e9b174 daemon \
-run-snark-worker $CODA_PUBLIC_KEY \
-snark-worker-fee 0.25 \
-work-selection seq \
-peer $SEED1 \
-peer $SEED2 \
-external-port 8305
```

Пример, как должна выглядеть команда:

{% code title="\#ПРИМЕР" %}
```text
sudo docker run -d \
--name coda-worker \
-p 8305:8305 \
--memory 16g \
--cpus 8 \
--restart always \
codaprotocol/coda-daemon:0.0.12-beta-feature-bump-genesis-timestamp-3e9b174 daemon \
-run-snark-worker $CODA_PUBLIC_KEY \
-snark-worker-fee 0.25 \
-work-selection seq \
-peer $SEED1 \
-peer $SEED2 \
-external-port 8305
```
{% endcode %}

## 2. Просмотр логов

Посмотреть запущенные контейнеры:

```text
sudo docker ps -a
```

Логи контейнера Производителя Блоков:

```text
sudo docker logs --follow coda -f
```

Логи контейнера Снарк Воркера:

```text
sudo docker logs --follow coda-worker -f
```

### Альтернативный вывод логов

```text
sudo docker exec coda coda client status | grep "Block producers"
```

Вывод покажет только строку с запущенным производителем блоков. Пример ниже:

{% code title="\#ПРИМЕР" %}
```text
root@Coda:~# sudo docker exec coda coda client status | grep "Block producers"
Block producers running:         1 (4vsRCVfshM6QYPWn8TFMLdYbCdf9abRW1t71dAjCXQPYURMmxVPFe4VjXfrxjYeFWEzMmqTpc8suhsRvA51NjvRe6rmWv9eerUjRJFjdRTWcoBdyuyDnGC3GbtKdWhv5b9CajERMD7PHj3z4)
```
{% endcode %}

![](../../.gitbook/assets/image.png)

## 3. Команды Докера

Остановка контейнера осуществляется командой:

```text
sudo docker stop coda
```

Удаление контейнера:

```text
sudo docker rm coda
```

Удаление запущенного контейнера:

```text
sudo docker rm -f coda
```



