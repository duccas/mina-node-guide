# Запуск Mina без Докера

{% hint style="warning" %}
Рекомендуем перед запуском использовать `tmux` для запуска нескольких сессий в одном терминале.
{% endhint %}

{% page-ref page="../nastroika-tmux.md" %}

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
brew install minaprotocol/mina/mina
```

Запуск ноды:

```text
brew services start mina
```

Если у вас уже установлен пакет `mina` его нужно обновить командой ниже. Если вы ранее не устанавливали `mina`, то можно эту команду не запускать.

```text
brew upgrade mina
```

## 3. Установка для `Ubuntu 18.04 / Debian 9`

Удаление предыдущих версий:

```text
sudo apt-get remove -y coda-testnet-postake-medium-curves
```

Создадим папку `.coda-config`:

```text
mkdir .coda-config
```

Скачиваем дистрибутив `Mina`:

```text
sudo apt-get update
sudo apt-get install -y apt-transport-https ca-certificates
echo "deb [trusted=yes] http://packages.o1test.net release main" | sudo tee /etc/apt/sources.list.d/mina.list
sudo apt-get update
sudo apt-get install -y curl mina-testnet-postake-medium-curves=0.1.1+-add-testworld-ledger-bbda99d --allow-downgrades
```

### 3.1 Запуск ноды

Производим командой:

```text
coda daemon \
-peer-list-file ~/peers.txt \
-generate-genesis-proof true \
-log-level Info
```

Перед запуском производителя нужно импортировать и разблокировать ключи:

```text
coda accounts import -privkey-path $KEYPATH
coda accounts unlock -public-key $MINA_PUBLIC_KEY
```

Запуск Производителя блоков:

```text
coda client set-staking -public-key $MINA_PUBLIC_KEY
```

Запуск Снарк Воркера:

```text
coda client set-snark-work-fee 0.025
coda client set-snark-worker -address $MINA_PUBLIC_KEY
```

Здесь вы можете установить комиссию Воркера `coda client set-snark-work-fee 0.025`, либо оставить как есть.

Далее переходим в следующий раздел и начинаем с Пункта 2:

{% page-ref page="../cli.-sozdanie-klyuchei-import-otpravka-tokenov.md" %}

