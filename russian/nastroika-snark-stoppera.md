# Настройка Снарк Стоппера

## Описание

Скрипт будет полезен тем, кто запускает Производителя блоков и Снарк Воркера на одном сервере. Воркер использует все ваши ядра процессора на 100%, что негативно сказывается на производителе блоков. Скрипт позволяет остановить Снарк Воркера за 3 минуты до производства блока и включит его опять через 10 минут.

{% hint style="info" %}
Данный скрипт создан пользователем @whataday2day\#1271  
[https://github.com/c29r3/coda-snark-stopper](https://github.com/c29r3/coda-snark-stopper)
{% endhint %}

## Подготовка

### Установка JSON

```text
sudo apt install jq -y
```

### **Установка Python 3.6**

Вы можете также установить Python 3.6 из Personal Package Archive J Fernyhough \(PPA\).  
Установить следующие требования

```text
sudo apt-get install software-properties-common python-software-properties
```

Выполните следующую команду, чтобы добавить PPA:

```text
sudo add-apt-repository ppa:jonathonf/python-3.6
```

Нажмите клавишу ВВОД, чтобы продолжить.

Обновите репозитории:

```text
sudo apt-get update
```

Наконец, установите Python версии 3.6:

```text
sudo apt-get install python3.6
```

После установки, вы можете проверить установленную версию, выполнив следующую команду:

```text
python3.6 -V
```

## Установка Снарк Стоппера

Устанавливаем пакет git, если он не установлен на сервере:

```text
yes | sudo apt install git
```

{% hint style="info" %}
Ваш snark worker должен быть ЗАПУЩЕН.
{% endhint %}

Проверьте файл конфигурации. Есть несколько вариантов, которые вы можете переназначить.

### Установка:

{% hint style="info" %}
Запускать нужно в новой сессии TMUX
{% endhint %}

```text
git clone https://github.com/c29r3/coda-snark-stopper.git \
&& cd coda-snark-stopper \
&& pip3 install -r requirements.txt \
&& python3 snark-stopper.py
```

Теперь нужно добавить в конфиг стоппера ваш публичный ключ и комиссию Воркера.  
Открываем конфиг командой:

```text
nano $HOME/coda-snark-stopper/config.yml
```

В строке `WORKER_PUB_KEY: YOUR_PUBLIC_KEY` измените `YOUR_PUBLIC_KEY` на `$MINA_PUBLIC_KEY`  
В строке `WORKER_FEE: 1` замените значение комиссии например с 1 на 0.25 \(значение должно быть такое же как в команде запуска ноды\)

Готово.

