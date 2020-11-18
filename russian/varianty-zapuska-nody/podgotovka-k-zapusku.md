# Подготовка публичных ключей

## 1. Подготовка публичных ключей

Перед запуском ноды нужно скопировать папку с ключами на ваш сервер, созданную ранее для регистрации в тестнет 4.1. 

{% hint style="info" %}
Этот пункт нужно выполнять в том случае, если при создании публичных ключей вы их сохранили на свой локальный компьютер и удалили сервер.   
Если вы не удаляли сервер и используете его же для запуска ноды, то вам следует перейти к пункту 1.2.
{% endhint %}

Используйте любой файловый менеджер \(Filezilla\) и подключитесь к своему серверу. Скопируйте в корень сервера папку `keys` .

![](../../.gitbook/assets/image%20%281%29.png)

### 1.2 Нужно дать папке права на запись и чтение:

```text
chmod 700 $HOME/keys
```

## 2. Экспорт ключей

Теперь запишем ваш публичный ключ на сервер в файл `.profile`, чтобы в следующий раз больше их не экспортировать\(пиры должны прислать на ваш email перед запуском тестнет 4.1\).

Замените `ВАШ ПАРОЛЬ`на пароль от публичного ключа.

```text
echo 'export KEYPATH=$HOME/keys/my-wallet' >> $HOME/.profile
echo 'export MINA_PUBLIC_KEY=$(cat $HOME/keys/my-wallet.pub)' >> $HOME/.profile
echo 'export CODA_PRIVKEY_PASS=ВАШ ПАРОЛЬ' >> $HOME/.profile
source ~/.profile
```

Пример, как это должно выглядеть:

{% code title="\#ПРИМЕР" %}
```text
echo 'export KEYPATH=$HOME/keys/my-wallet' >> $HOME/.profile
echo 'export MINA_PUBLIC_KEY=$(cat $HOME/keys/my-wallet.pub)' >> $HOME/.profile
echo 'export CODA_PRIVKEY_PASS=qwerty123' >> $HOME/.profile
source ~/.profile
```
{% endcode %}

Экспортируем.

