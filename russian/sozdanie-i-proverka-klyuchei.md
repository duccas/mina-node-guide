# Создание и проверка ключей

## Установка

### Установка пакетов

```text
echo "deb [trusted=yes] http://packages.o1test.net release main" | sudo tee /etc/apt/sources.list.d/mina.list
sudo apt-get update
sudo apt-get install -y curl unzip mina-testnet-postake-medium-curves=0.4.2-245a3f7
```

### Установка генератора ключей

```text
sudo apt-get install mina-generate-keypair=0.2.12-718eba4
```

## Генерируем ключи

При создании ключей вас попросят создать пароль.

### Вариант 1 с помощью пакета:

```text
mina-generate-keypair -privkey-path ~/keys/my-wallet
```

### Вариант 2 с помощью докера:

```text
docker run  --interactive --tty --rm --volume $(pwd)/keys:/keys minaprotocol/generate-keypair:0.2.12-718eba4 -privkey-path /keys/my-wallet
```

### Устанавливаем права:

```text
chmod 700 $HOME/keys
chmod 600 $HOME/keys/my-wallet
```

## Проверка ключей

При проверке ключа вас попросят ввести пароль от него.

### Вариант 1 с помощью пакета:

```text
mina-validate-keypair -privkey-path ~/keys/my-wallet
```

### Вариант 2 с помощью докера:

```text
docker run --interactive --tty --rm --entrypoint=mina-validate-keypair --volume $(pwd)/keys:/keys minaprotocol/generate-keypair:0.2.12-718eba4 -privkey-path /keys/my-wallet
```

В обоих случаях, если все в порядке с вашими ключами вы получите сообщение:

```text
Verified a transaction using specified keypair
```

Оно означает, что ваши ключи проверены.

