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

## 3. Подготовка для `Ubuntu 18.04 / Debian 9`

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
sudo apt-get install -y curl unzip mina-testnet-postake-medium-curves=0.2.2-1-b14e324 --allow-downgrades
```

## 4. Варианты запуска

### 4.1 Запуск в Сервисе

Настройка файла с флагами:

```text
mkdir $HOME/.mina-env && nano $HOME/.mina-env
```

Копируем и вставляем переменные в файл предварительно вписав ваш пароль от ключа вместо `ВАШ ПАРОЛЬ ДЛЯ КЛЮЧА`:

```text
CODA_PRIVKEY_PASS="ВАШ ПАРОЛЬ ДЛЯ КЛЮЧА"
EXTRA_FLAGS=" -file-log-level Info -super-catchup "
```

Сохраняем и выходим: CTRL+S и CTRL+X

### 4.1.1 Добавление флагов Снарк Воркера \(если нужно\)

Добавляем в файл `.mina-env` флаги Снарк воркера с вашим ключем и комиссией:

```text
EXTRA_FLAGS=" -snark-worker-fee 0.025 -run-snark-worker B62qkWFkU9PDSzAxWWXVcxxHe1nJnfGqLeYbtxDLv5BxPiekGcxLTpj -work-selection seq -file-log-level Info -super-catchup "
```

По умолчанию `-work-selection` для Снарк Воркера является случайным `rand`.  
Вы можете изменить это, добавив флаг `-work-selection seq` в конец команды запуска, которая будет работать с заданиями в том порядке, в котором они должны быть включены из состояния сканирования и скорее всего приведет к включению ваших снарков без потенциально длительной задержки.

### 4.2 Запускаем сервис

```text
systemctl --user daemon-reload
systemctl --user start mina
systemctl --user enable mina
sudo loginctl enable-linger
```

Просмотр логов:

```text
journalctl --user-unit mina -n 1000 -f
```

### 4.3 Запуск ноды в TMUX

Запускаем пустую сессию в Tmux:

```text
tmux new -s session
```

Подробнее о TMUX:

{% page-ref page="../nastroika-tmux.md" %}

И запуск в сессии производим командой:

```text
coda daemon \
-peer-list-file ~/peers.txt \
-generate-genesis-proof true \
-log-level Info \
-super-catchup
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

