# Подготовка публичных ключей

## 1. Подготовка публичных ключей

Перед запуском ноды нужно скопировать папку с ключами на ваш сервер, созданную ранее для регистрации в тестнет 3.3. 

{% hint style="info" %}
Этот пункт нужно выполнять в том случае, если при создании публичных ключей вы их сохранили на свой локальный компьютер и удалили сервер.   
Если вы не удаляли сервер и используете его же для запуска ноды, то вам следует перейти к пункту 1.2.
{% endhint %}

Используйте любой файловый менеджер \(Filezilla\) и подключитесь к своему серверу. Скопируйте в корень сервера папку `keys` .

![](../../.gitbook/assets/image%20%281%29.png)

### 1.2 Нужно дать папкам и файлам права на запись и чтение:

```text
chmod 700 $HOME/keys
chmod 600 $HOME/keys/my-wallet
```

## 2. Экспорт ключей и SEED пиров

Теперь запишем ваш публичный ключ и SEED пиры на сервер в файл `.profile`, чтобы в следующий раз больше их не экспортировать\(пиры должны прислать на ваш email перед запуском тестнет 3.3\).

Замените `ВАШ ПАРОЛЬ` на пароль от публичного ключа.

```text
echo 'export CODA_PUBLIC_KEY=$(cat $HOME/keys/my-wallet.pub)' >> $HOME/.profile
echo 'export SEED1=ВАШ СИД' >> $HOME/.profile
echo 'export CODA_PASS=ВАШ ПАРОЛЬ' >> $HOME/.profile
source .profile
```

Пример, как это должно выглядеть:

{% code title="\#ПРИМЕР" %}
```text
echo 'export CODA_PUBLIC_KEY=$(cat $HOME/keys/my-wallet.pub)' >> $HOME/.profile
echo 'export SEED1=/dns1/o1test.net/tcp/10001/p2p/1548HgtdaFq2yEQFFzhU5dt64AWqawRuomG9hL8rSmm5vDyajjkf' >> $HOME/.profile
echo 'export CODA_PASS=qwerty123' >> $HOME/.profile
source .profile
```
{% endcode %}

Экспортируем.

