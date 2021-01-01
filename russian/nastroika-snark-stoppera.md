# Настройка Снарк Стоппера

## Описание

Скрипт будет полезен тем, кто запускает Производителя блоков и Снарк Воркера на одном сервере. Воркер использует все ваши ядра процессора на 100%, что негативно сказывается на производителе блоков. Скрипт позволяет остановить Снарк Воркера за 3 минуты до производства блока и включит его опять через 10 минут.

{% hint style="info" %}
Данный скрипт создан пользователем @whataday2day\#1271  
[https://github.com/c29r3/mina-snark-stopper](https://github.com/c29r3/mina-snark-stopper)
{% endhint %}

## Подготовка

### Установка JSON

```text
sudo apt install jq -y
```

### Устанавливаем пакет git, если он не установлен на сервере:

```text
yes | sudo apt install git
```

{% hint style="info" %}
Ваш snark worker должен быть ЗАПУЩЕН.
{% endhint %}

Проверьте файл конфигурации. Есть несколько вариантов, которые вы можете переназначить.

Разблокируем приватную сеть:

```text
sudo iptables -D OUTPUT -p tcp -d 172.16.0.0/12 -j DROP
```

## 1. Установка без Докера:

{% hint style="info" %}
Запускать нужно в новой сессии TMUX
{% endhint %}

```text
sudo apt-get update \
&& sudo apt-get install python3-venv tmux git -y \
&& git clone https://github.com/c29r3/mina-snark-stopper.git \
&& cd mina-snark-stopper \
&& python3 -m venv venv \
&& source ./venv/bin/activate \
&& pip3 install -r requirements.txt
```

Теперь нужно добавить в конфиг стоппера ваш публичный ключ и комиссию Воркера.  
Открываем конфиг командой:

```text
nano $HOME/mina-snark-stopper/config.yml
```

В строке `WORKER_PUB_KEY: YOUR_PUBLIC_KEY` измените `YOUR_PUBLIC_KEY` на `$MINA_PUBLIC_KEY`  
В строке `WORKER_FEE: 1` замените значение комиссии например с 1 на 0.25 \(1000000000 на 25000000\)

### 1.1 Запуск

```text
tmux new -s snark-stopper -d venv/bin/python3 snark-stopper.py
```

Просмотр логов:

```text
tmux attach -t snark-stopper
```

## 2. Установка с Докером:

Скачиваем файл с конфигом:

```text
wget https://raw.githubusercontent.com/c29r3/mina-snark-stopper/master/config.yml
```

Теперь нужно добавить в конфиг стоппера ваш публичный ключ и комиссию Воркера.  
Открываем конфиг командой:

```text
nano $HOME/config.yml
```

В строке `WORKER_PUB_KEY: YOUR_PUBLIC_KEY` измените `YOUR_PUBLIC_KEY` на `$MINA_PUBLIC_KEY`  
В строке `WORKER_FEE: 1` замените значение комиссии например с 1 на 0.25 \(1000000000 на 25000000\)

{% hint style="info" %}
1 MINA = 1,000,000,000 nanomina
{% endhint %}

Запускаем контейнер:

```text
docker run -d \
--volume $(pwd)/config.yml:/mina/config.yml \
--net=host \
--restart always \
--name snark-stopper \
c29r3/snark-stopper
```

Просмотр логов:

```text
docker logs -f snark-stopper
```

### 3. Решения ошибок

Если снарк-стоппер не может подключиться к порту `3085`:

Нужно проверить доступность порта:

```text
nc -t -vv localhost 3085
```

Результат должен быть примерно таким:  
`Connection to localhost 3085 port [tcp/*] succeeded!`

Если соединение зависает, то возможны следующие варианты:

* Доступ к порту `3085` заблокирован через `ufw\iptables`
* Вы не добавили флаг контейнера докеров `-p 127.0.0.1:3085:3085`
* Узел еще не синхронизирован. По этой причине стоппер не может подключиться

Порт отвечает, но стоппер по-прежнему не может подключиться:

```text
iptables -D OUTPUT -p tcp -d 172.16.0.0/12 -j DROP
```

это из-за блокировки частных подсетей, которые использует докер.

#### Обновить образ докера

После выполнения приведенной ниже команды перейдите к шагу 2.

```text
sudo docker rm -f snark-stopper \
&& sudo docker pull c29r3/snark-stopper
```

### 4. Обновление

Удаляем файл с конфигом и контейнер и скачиваем новый образ:

```text
rm config.yml \
&& sudo docker rm -f snark-stopper \
&& sudo docker pull c29r3/snark-stopper
```

Далее продолжаем с пункта 1 или 2.

### 5. Удаление

```text
rm -rf mina-snark-stopper \
&& sudo docker rm -f snark-stopper \
&& sudo docker system prune -af
```

