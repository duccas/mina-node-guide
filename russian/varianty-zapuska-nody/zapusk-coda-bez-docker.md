# Запуск Coda без Докера

{% hint style="warning" %}
Рекомендуем перед запуском использовать `tmux` для запуска нескольких сессий в одном терминале.
{% endhint %}

## 1. Настройка Фаервола

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

## 2. Установка для macOS

Нужно установить [Homebrew](https://brew.sh/).

```text
brew install wget
```

Установка пакетов `coda`.

```text
brew install codaprotocol/coda/coda
```

Запуск ноды:

```text
brew services start coda
```

Если у вас уже установлен пакет `coda` его нужно обновить командой ниже. Если вы ранее не устанавливали `coda`, то можно эту команду не запускать.

```text
brew upgrade coda
```

## 3. Установка для `Ubuntu 18.04 / Debian 9`

Удаление предыдущих версий:

```text
sudo apt-get remove coda-testnet-postake-medium-curves
sudo apt-get remove coda-kademlia
```

Скачиваем дистрибутив `Coda`:

```text
echo "deb [trusted=yes] http://packages.o1test.net release main" | sudo tee /etc/apt/sources.list.d/coda.list
sudo apt-get update
sudo apt-get install coda-testnet-postake-medium-curves
```

### 3.1 Запуск ноды

Производим командой:

```text
coda daemon \
-peer $SEED1
```

Запуск Производителя блоков:

```text
coda client set-staking -public-key $CODA_PUBLIC_KEY
```

Запуск Снарк Воркера:

```text
coda client set-snark-work-fee 0.25
coda client set-snark-worker -address $CODA_PUBLIC_KEY
```

Здесь вы можете установить комиссию Воркера `coda client set-snark-work-fee 0.25`, либо оставить как есть.

Далее переходим в следующий раздел и начинаем с Пункта 2:

{% page-ref page="../cli.-sozdanie-klyuchei-import-otpravka-tokenov.md" %}

## 4. Альтернативный запуск Производителя Блоков и Снарк Воркера.

### 4.1 Запуск Производителя блоков \(Block Producer\):

```text
coda daemon \
-peer $SEED1 \
-block-producer-pubkey $CODA_PUBLIC_KEY
```

### 4.2  Запуск Снарк Воркера \(Snark Worker\):

Описание изменяемых переменных:

`-snark-worker-fee 0.25` - нужно установить комиссию воркера, либо оставить так, как есть

```text
coda daemon \
-peer $SEED1 \
-run-snark-worker $CODA_PUBLIC_KEY \
-snark-worker-fee 0.25 
```

