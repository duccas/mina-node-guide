# Создание и проверка ключей

## 1. Установка

### Установка пакетов

```
echo "deb [trusted=yes] http://packages.o1test.net $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/mina.list
sudo apt-get update
sudo apt-get install -y curl unzip mina-mainnet=1.3.0-9b0369c
```

### Установка генератора ключей

```
sudo apt-get install mina-generate-keypair=1.3.0-9b0369c
```

## 2. Создание ключей

При создании ключей вас попросят создать пароль.

### Вариант 1 с помощью пакета:

```
mina-generate-keypair -privkey-path ~/keys/my-wallet
```

### Вариант 2 с помощью докера:

```
sudo docker run  --interactive --tty --rm --volume $(pwd)/keys:/keys minaprotocol/mina-generate-keypair:1.3.0-9b0369c --privkey-path /keys/my-wallet
```

### Устанавливаем права:

```
chmod 700 $HOME/keys
chmod 600 $HOME/keys/my-wallet
```

## 3. Проверка ключей

При проверке ключа вас попросят ввести пароль от него.

### Вариант 1 с помощью пакета:

```
mina-validate-keypair -privkey-path ~/keys/my-wallet
```

### Вариант 2 с помощью докера:

```
sudo docker run --interactive --tty --rm --entrypoint=mina-validate-keypair --volume $(pwd)/keys:/keys minaprotocol/mina-generate-keypair:1.3.0-9b0369c --privkey-path /keys/my-wallet
```

В обоих случаях, если все в порядке с вашими ключами вы получите сообщение:

```
Verified a transaction using specified keypair
```

Оно означает, что ваши ключи проверены.

## 4. Экспорт ключей

Теперь запишем ваш публичный ключ на сервер в файл `.bashrc`, чтобы в следующий раз больше их не экспортировать.

```
echo 'export KEYPATH=$HOME/keys/my-wallet' >> $HOME/.bashrc
echo 'export MINA_PUBLIC_KEY=$(cat $HOME/keys/my-wallet.pub)' >> $HOME/.bashrc
source ~/.bashrc
```
