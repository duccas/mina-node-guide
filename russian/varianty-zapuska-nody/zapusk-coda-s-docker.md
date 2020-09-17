# Запуск Coda с Докером

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

Для того, чтобы Докер мог увидеть открытые порты через UFW нужно сделать следующее.   
Вводим команду: 

```text
sudo nano /etc/default/docker
```

Затем вставляем строку `DOCKER_OPTS="--iptables=false"` в открывшийся файл.  
Сохраняем `CTRL+C` и закрываем `CTRL+X`.  
  
И перезагружаем Докер:

```text
sudo systemctl restart docker
```

Открываем порты 22, 8302 и 8303 и активируем Firewall:

```text
sudo ufw allow 22 \
&& sudo ufw allow 8302 \
&& sudo ufw allow 8303 \
&& yes | sudo ufw enable
```

Проверяем статус открытых портов командой:

```text
sudo ufw status
```

{% hint style="info" %}
Если у вас на сервере не установлен UFW, установите его используя команду `sudo apt install ufw`
{% endhint %}

## 2. Варианты запуска ноды

{% hint style="warning" %}
Выберите только один вариант запуска из 2-х предложенных ниже \(пункт 2.1 или 2.2\).
{% endhint %}

### 2.1 Запуск только Производителя блоков \(Block Producer\):

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
codaprotocol/coda-daemon:0.0.16-beta6 daemon \
-block-producer-key $HOME/keys/my-wallet \
-peer $SEED1
```

### 2.2 Запуск Производителя блоков \(Block Producer\) вместе со Снарк Воркером \(Snark Worker\):

{% hint style="warning" %}
Если вы не хотите запускать Snark Worker. Вы можете сразу перейти к шагу 3.
{% endhint %}

Для начала нужно закрыть порт 3085:

```text
sudo ufw deny 3085
```

Описание изменяемых переменных:

1. `--name coda` - имя для контейнера можно использовать любое, либо оставить так, как есть
2. `--memory 16g` - ограничение количества оперативной памяти, которое может использовать контейнер
3. `--cpus 8` - ограничение количества ядер процессора, которые может использовать контейнер
4. `-snark-worker-fee 0.25` - можно установить комиссию Снарк Воркера

Нужно внести все данные в команду ниже:

```text
sudo docker run -d \
-e "CODA_PRIVKEY_PASS=$CODA_PASS" \
--mount type=bind,source="$(pwd)"/keys,target=/root/keys \
--name coda \
-p 8302:8302 \
-p 8303:8303 \
-p 127.0.0.1:3085:3085 \
--memory 16g \
--cpus 8 \
--restart always \
codaprotocol/coda-daemon:0.0.16-beta6 daemon \
-peer $SEED1 \
-block-producer-key $HOME/keys/my-wallet \
-run-snark-worker $CODA_PUBLIC_KEY \
-snark-worker-fee 0.25 \
-work-selection seq
```

{% hint style="info" %}
Для работы одновременно Производителя блоков \(Block Producer\) и Снарк Воркера \(Snark Worker\) нужно настраивать Снарк Стоппер. Чтобы ненадолго останавливать Воркера во время производства блока.

Перейдите по ссылке ниже, чтобы настроить Снарк Стоппер.
{% endhint %}

{% page-ref page="../nastroika-snark-stoppera.md" %}

## 3. Просмотр логов

Посмотреть запущенные контейнеры:

```text
sudo docker ps -a
```

Логи контейнера Производителя Блоков:

```text
sudo docker logs --follow coda -f
```

### 3.1 Альтернативный вывод логов

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

## 4. Команды Докера

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

