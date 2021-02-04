# Подготовка публичных ключей и пиров

## 1. Подготовка публичных ключей

Перед запуском ноды нужно скопировать ключи на ваш сервер. 

Удалим старую папку с ключами и создадим новую:

```text
rm -rf ~/keys
mkdir ~/keys
```

Ссылка на скачивание архива с ключами придет на ваш емейл. 

Замените `<ВАША ССЫЛКА>` на ссылку из письма.

```text
wget -O ~/keys/new-keys.zip <ВАША ССЫЛКА>
```

Пример готовой команды:

{% code title="\#ПРИМЕР" %}
```text
wget -O ~/keys/new-keys.zip https://keys.com/keys/12031398-0-0dskfhskjdfh12313.zip
```
{% endcode %}

Зайдем в папку `keys` и распакуем скачанные файлы:

```text
cd ~/keys
unzip new-keys.zip
```

В результате мы увидим, что распакованы 2 файла с именами `extra_fish_account_1937` и `extra_fish_account_1937.pub`.

![](../../.gitbook/assets/image%20%284%29.png)

Теперь эти файлы нужно переименовать в правильный вид. Замените в командах ниже `<НАЗВАНИЕ ФАЙЛА>` на названия файлов после распаковки.

```text
mv <НАЗВАНИЕ ФАЙЛА> my-wallet
mv <НАЗВАНИЕ ФАЙЛА>.pub my-wallet.pub
```

Выглядеть будет вот так:

{% code title="\#ПРИМЕР" %}
```text
mv extra_fish_account_1937 my-wallet
mv extra_fish_account_1937.pub my-wallet.pub
```
{% endcode %}

Выходим из папки `keys`:

```text
cd
```

Вставляем в терминал. Готово.

### 1.2 Нужно дать папке права на запись и чтение:

```text
chmod 700 $HOME/keys
chmod 600 $HOME/keys/my-wallet
```

## 2. Экспорт ключей

Теперь запишем ваш публичный ключ на сервер в файл `.bashrc`, чтобы в следующий раз больше их не экспортировать.

```text
echo 'export KEYPATH=$HOME/keys/my-wallet' >> $HOME/.bashrc
echo 'export MINA_PUBLIC_KEY=$(cat $HOME/keys/my-wallet.pub)' >> $HOME/.bashrc
source ~/.bashrc
```

Пример, как это должно выглядеть:

{% code title="\#ПРИМЕР" %}
```text
echo 'export KEYPATH=$HOME/keys/my-wallet' >> $HOME/.bashrc
echo 'export MINA_PUBLIC_KEY=$(cat $HOME/keys/my-wallet.pub)' >> $HOME/.bashrc
source ~/.bashrc
```
{% endcode %}

Экспортируем.

## 3. Подготовка пиров

Перед запуском тестнета Testworld участникам на email должна прийти ссылка на скачивание пиров для запуска ноды.   
Ссылка на скачивание пиров:

```text
wget -O ~/peers.txt https://raw.githubusercontent.com/MinaProtocol/mina/encore-peers/automation/terraform/testnets/encore/peers.txt
```

Вставляем в терминал и скачиваем.  
Готово.

