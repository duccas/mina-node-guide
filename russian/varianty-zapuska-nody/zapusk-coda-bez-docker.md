# Запуск Coda без Докера

{% hint style="warning" %}
Рекомендуем перед запуском использовать `tmux` для запуска нескольких сессий в одном терминале.
{% endhint %}

## 1. Установка для macOS

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

## 2. Установка для `Ubuntu 18.04 / Debian 9`

Удаление предыдущих версий:

```text
sudo apt-get remove coda-testnet-postake-medium-curves
sudo apt-get remove coda-kademlia
```

Скачиваем дистрибутив `Coda`:

```text
sudo apt update
echo "deb [trusted=yes] http://packages.o1test.net unstable main" | sudo tee /etc/apt/sources.list.d/coda.list
sudo apt-get install -t coda-testnet-postake-medium-curves=0.0.12-beta+406048-feature-bump-genesis-timestamp-3e9b174-PV48525e92
```

### 2.1 Запуск Производителя блоков \(Block Producer\):

```text
coda daemon \
-peer $SEED1 \
-peer $SEED2 \
-block-producer-pubkey $CODA_PUBLIC_KEY
```

### 2.2 Запуск Снарк Воркера \(Snark Worker\):

Описание изменяемых переменных:

`-snark-worker-fee 0.25` - нужно установить комиссию воркера, либо оставить так, как есть

```text
coda daemon \
-peer $SEED1 \
-peer $SEED2 \
-run-snark-worker $CODA_PUBLIC_KEY \
-snark-worker-fee 0.25 
```

