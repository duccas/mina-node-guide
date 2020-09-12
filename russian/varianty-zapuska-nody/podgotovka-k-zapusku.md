# Подготовка к запуску

## 1. Подготовка

### Настройка Ubuntu

Обновляем пакеты на сервере до новейших версий:

```text
sudo apt update
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

### Настройка Фаервола

Открываем порты 22, 8302 и 8303 и активируем Firewall:

```text
sudo ufw allow 22 && sudo ufw allow 8302 && sudo ufw allow 8303 && yes | sudo ufw enable
```

Проверяем статус открытых портов командой:

```text
sudo ufw status
```

{% hint style="info" %}
Если у вас на сервере не установлен UFW, установите его используя команду `sudo apt install ufw`
{% endhint %}

## 1.1 Подготовка публичных ключей

Перед запуском ноды нужно скопировать папку с ключами на ваш сервер, созданную ранее для регистрации в тестнет 3.3. 

{% hint style="info" %}
Этот пункт нужно выполнять в том случае, если при создании публичных ключей вы их сохранили на свой локальный компьютер и удалили сервер.   
Если вы не удаляли сервер и используете его же для запуска ноды, то вам следует перейти к пункту 1.1.2.
{% endhint %}

Используйте любой файловый менеджер \(Filezilla\) и подключитесь к своему серверу. Скопируйте в корень сервера папку `keys` .

![](../../.gitbook/assets/image%20%281%29.png)

#### 1.1.2 Нужно дать папкам и файлам права на запись и чтение:

```text
chmod 700 keys
chmod 600 keys/my-wallet
```

## 1.2 Экспорт ключей и SEED пиров

Теперь запишем ваш публичный ключ и SEED пиры на сервер в файл `.profile`, чтобы в следующий раз больше их не экспортировать\(пиры должны прислать на ваш email перед запуском тестнет 3.3\).

```text
echo 'export CODA_PUBLIC_KEY=$(cat $HOME/keys/my-wallet.pub)' >> $HOME/.profile
echo 'export SEED1=/ip4/34.74.183.100/tcp/10001/ipfs/12D3KooWAFFq2yEQFFzhU5dt64AWqawRuomG9hL8rSmm5vxhAsgr' >> $HOME/.profile
echo 'export SEED2=/ip4/35.231.128.243/tcp/10001/ipfs/12D3KooWB79AmjiywL1kMGeKHizFNQE9naThM2ooHgwFcUzt6Yt1' >> $HOME/.profile
source .profile
```

Делаем экспорт ключа и SEED пиров:

```text
export CODA_PUBLIC_KEY=$(cat $HOME/keys/my-wallet.pub)
export SEED1=/ip4/34.74.183.100/tcp/10001/ipfs/12D3KooWAFFq2yEQFFzhU5dt64AWqawRuomG9hL8rSmm5vxhAsgr
export SEED2=/ip4/35.231.128.243/tcp/10001/ipfs/12D3KooWB79AmjiywL1kMGeKHizFNQE9naThM2ooH
```

