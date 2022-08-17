# Запуск Mina без Докера

{% hint style="warning" %}
Рекомендуем перед запуском использовать `tmux` для запуска нескольких сессий в одном терминале.
{% endhint %}

{% content-ref url="../nastroika-tmux.md" %}
[nastroika-tmux.md](../nastroika-tmux.md)
{% endcontent-ref %}

## 1. Настройка Фаервола

Открываем порты 22, 8302 и 8303 и активируем Firewall:

```
sudo ufw allow 22 \
&& sudo ufw allow 8302 \
&& sudo ufw allow 8303 \
&& yes | sudo ufw enable
```

Проверяем статус открытых портов командой:

```
sudo ufw status
```

{% hint style="info" %}
Если у вас на сервере не установлен UFW, установите его используя команду `sudo apt install ufw`
{% endhint %}

## 2. Установка для macOS

Нужно установить [Homebrew](https://brew.sh/).

```
brew install wget
```

Установка пакетов `coda`.

```
brew install minaprotocol/mina/mina
```

Запуск ноды:

```
brew services start mina
```

Если у вас уже установлен пакет `mina` его нужно обновить командой ниже. Если вы ранее не устанавливали `mina`, то можно эту команду не запускать.

```
brew upgrade mina
```

## 3. Подготовка для `Ubuntu 18.04 / Debian 9`

Создадим папку `.coda-config`:

```
mkdir .mina-config
```

Скачиваем дистрибутив `Mina`:

```
echo "deb [trusted=yes] http://packages.o1test.net $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/mina.list
sudo apt-get update
sudo apt-get install -y curl unzip mina-mainnet=1.3.0-9b0369c
```

## 4. Варианты запуска

### 4.1 Запуск в Сервисе

Настройка файла с флагами:

```
nano .mina-env
```

Копируем и вставляем переменные в файл предварительно вписав ваш пароль от ключа вместо `ВАШ ПАРОЛЬ ДЛЯ КЛЮЧЕЙ и ПУТЬ_К_КЛЮЧУ`:

```
MINA_PRIVKEY_PASS="ВАШ ПАРОЛЬ ДЛЯ КЛЮЧА"
LOG_LEVEL=Info
FILE_LOG_LEVEL=Debug
EXTRA_FLAGS=" --block-producer-key $ПУТЬ_К_КЛЮЧУ"
PEER_LIST_URL=https://storage.googleapis.com/mina-seed-lists/mainnet_seeds.txt
```

Сохраняем и выходим: CTRL+S и CTRL+X

### 4.1.1 Добавление флагов Снарк Воркера (если нужно)

Добавляем в файл `.mina-env` флаги Снарк воркера с вашим ключем и комиссией:

```
EXTRA_FLAGS=" -snark-worker-fee 0.025 -run-snark-worker B62qkWFkU9PDSzAxWWXVcxxHe1nJnfGqLeYbtxDLv5BxPiekGcxLTpj -work-selection seq
```

По умолчанию `-work-selection` для Снарк Воркера является случайным `rand`.\
Вы можете изменить это, добавив флаг `-work-selection seq` в конец команды запуска, которая будет работать с заданиями в том порядке, в котором они должны быть включены из состояния сканирования и скорее всего приведет к включению ваших снарков без потенциально длительной задержки.

### 4.1.2 Запускаем сервис

```
systemctl --user daemon-reload
systemctl --user start mina
systemctl --user enable mina
sudo loginctl enable-linger
```

Просмотр логов:

```
journalctl --user-unit mina -n 1000 -f
```

### 4.2 Запуск ноды в TMUX

Запускаем пустую сессию в Tmux:

```
tmux new -s session
```

Подробнее о TMUX:

{% content-ref url="../nastroika-tmux.md" %}
[nastroika-tmux.md](../nastroika-tmux.md)
{% endcontent-ref %}

И запуск в сессии производим командой:

```
mina daemon \
--peer-list-url https://storage.googleapis.com/mina-seed-lists/mainnet_seeds.txt \
--generate-genesis-proof true
```

**Выходим из сессии командой CTRL+B и теперь жмем D.**

Перед запуском производителя нужно импортировать и разблокировать ключи:

```
mina accounts import -privkey-path $KEYPATH
mina accounts unlock -public-key $MINA_PUBLIC_KEY
```

Запуск Производителя блоков:

```
mina client set-staking -public-key $MINA_PUBLIC_KEY
```

Запуск Снарк Воркера:

```
mina client set-snark-work-fee 0.025
mina client set-snark-worker -address $MINA_PUBLIC_KEY
```

Здесь вы можете установить комиссию Воркера `mina client set-snark-work-fee 0.025`, либо оставить как есть.

Далее переходим в следующий раздел и начинаем с Пункта 2:

{% content-ref url="../cli.-sozdanie-klyuchei-import-otpravka-tokenov.md" %}
[cli.-sozdanie-klyuchei-import-otpravka-tokenov.md](../cli.-sozdanie-klyuchei-import-otpravka-tokenov.md)
{% endcontent-ref %}
