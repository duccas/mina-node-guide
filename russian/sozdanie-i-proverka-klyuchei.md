# Создание и проверка ключей

## 1. Установка

### Установка пакетов

```text
echo "deb [trusted=yes] http://packages.o1test.net release main" | sudo tee /etc/apt/sources.list.d/mina.list
sudo apt-get update
sudo apt-get install -y curl unzip mina-mainnet=1.1.5-a42bdee
```

### Установка генератора ключей

```text
sudo apt-get install mina-generate-keypair=1.1.5-a42bdee
```

## 2. Создание ключей

При создании ключей вас попросят создать пароль.

### Вариант 1 с помощью пакета:

```text
mina-generate-keypair -privkey-path ~/keys/my-wallet
```

### Вариант 2 с помощью докера:

```text
sudo docker run  --interactive --tty --rm --volume $(pwd)/keys:/keys minaprotocol/generate-keypair:1.0.2-06f3c5c -privkey-path /keys/my-wallet
```

### Устанавливаем права:

```text
chmod 700 $HOME/keys
chmod 600 $HOME/keys/my-wallet
```

## 3. Проверка ключей

При проверке ключа вас попросят ввести пароль от него.

### Вариант 1 с помощью пакета:

```text
mina-validate-keypair -privkey-path ~/keys/my-wallet
```

### Вариант 2 с помощью докера:

```text
sudo docker run --interactive --tty --rm --entrypoint=mina-validate-keypair --volume $(pwd)/keys:/keys minaprotocol/generate-keypair:1.0.2-06f3c5c -privkey-path /keys/my-wallet
```

В обоих случаях, если все в порядке с вашими ключами вы получите сообщение:

```text
Verified a transaction using specified keypair
```

Оно означает, что ваши ключи проверены.

## 4. Экспорт ключей

Теперь запишем ваш публичный ключ на сервер в файл `.bashrc`, чтобы в следующий раз больше их не экспортировать.

```text
echo 'export KEYPATH=$HOME/keys/my-wallet' >> $HOME/.bashrc
echo 'export MINA_PUBLIC_KEY=$(cat $HOME/keys/my-wallet.pub)' >> $HOME/.bashrc
source ~/.bashrc
```

