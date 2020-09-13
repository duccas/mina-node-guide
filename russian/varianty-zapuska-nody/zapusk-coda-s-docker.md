# Запуск Coda с Докером

## 1. Варианты запуска ноды

{% hint style="warning" %}
ВАЖНО! Внимательно прочитайте каждый пункт гайда, чтобы не было проблем с запуском.
{% endhint %}

### Запуск Производителя блоков \(Block Producer\):

Описание изменяемых переменных:

`--name coda` - имя для контейнера можно использовать любое, либо оставить так, как есть

```text
sudo docker run -d \
-e "CODA_PRIVKEY_PASS=$CODA_PASS" \
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

Нужно внести все данные в команду ниже:

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

```text
sudo docker stop coda-worker
```

Удаление контейнера:

```text
sudo docker rm coda
```

```text
sudo docker rm coda-worker
```

Удаление запущенного контейнера:

```text
sudo docker rm -f coda
```

```text
sudo docker rm -f coda-worker
```

