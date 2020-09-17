# CLI. Импорт ключа, отправка токенов

## Использование CLI

### 1. Для доступа к командам [Coda CLI](https://codaprotocol.com/docs/cli-reference):

{% hint style="warning" %}
Этот шаг нужно выполнять только если вы запускали ноду с помощью Докера. Если же вы не использовали Докер, переходите сразу к шагу 2.
{% endhint %}

```text
sudo docker exec -it coda bash
```

Выход из CLI осуществляется командой:

```text
exit
```

### 2. Проверка статуса, состояния узла:

```text
coda client status
```

Выглядит это вот так:

![](../.gitbook/assets/image%20%283%29.png)

Дождитесь пока нода синхронизируется. В поле `Sync status:` должно быть написано `Synced`. Если в статусе написано `Catched`, значит нужно подождать чуть больше.  
После этого можно приступать к импорту ваших ключей.

### 3. Импортируем ключи

 Импорт аккаунта с ключем производим следующей командой:

```text
coda accounts import -privkey-path $HOME/keys/my-wallet
```

Список ваших аккаунтов можно посмотреть командой ниже:

```text
coda accounts list
```

### 4. Разблокировка аккаунта:

Для начала экспортируем Публичный ключ:

```text
export CODA_PUBLIC_KEY=$(cat $HOME/keys/my-wallet.pub)
```

Разблокируем аккаунт, чтобы можно было перемещать токены:

```text
coda accounts unlock -public-key $CODA_PUBLIC_KEY
```

В поле ввода пароля пишем ваш пароль от ключа и жмем ENTER.

### 5. Создание токенов

Теперь создадим токены:

```text
coda client create-token \
-sender $CODA_PUBLIC_KEY
```

В ответ мы получим следующее:

{% code title="\#ПРИМЕР ОТВЕТА " %}
```text
Dispatched create new token command with ID 2cUDm3QoJ14znWj5LxN8hjwwuvtwi9FGXcy56i4dPvYizPNwr3V9c9ePYxy3pJKjgogNx28jwHhqupi6wHFgXBmU5iX27iK1zUvJarj6wJsUG8segWXc4LGPed66YbYk3u9HiWw4v8cYYEqcy1mU6hqfj5JPMPthEBifxUMHZTqCwZmYWSdiERxB6PtPEdXVraWaYPVU4Q8vtpSN7oSTK1AXyXLYYR835CBrNSmgbLvoBDNroCKwcQrzw4b76BFNLe6EuWvBcMgX6npeeAbPg8z8iJ4PKz3gA64o1Y72kCrqyqus718LwXcmp5jxsYvJB2CJHzyZ
```
{% endcode %}

### 6. Получим ID токенов

Чтобы проводить следующие операции нам нужно знать ID токенов. Получим его следующей командой:

```text
coda client get-tokens \
-public-key $CODA_PUBLIC_KEY
```

В ответ мы получим следующее:

{% code title="\#ПРИМЕР ОТВЕТА" %}
```text
Accounts are held for token IDs:
1
```
{% endcode %}

### 7. Проверим баланс токенов

Баланс токенов проверяем командой ниже с вашим ID токенов:

```text
coda client get-balance \
-public-key $CODA_PUBLIC_KEY
```

Мы увидим баланс coda токенов.

### 8. Минт токенов

Чтобы сминтить новые токены нужно выполнить команду `mint-tokens`. Будут созданы 1,000 токенов в учетной записи отправителя транзакции под номером token ID 2.

```text
coda client mint-tokens \
-sender $CODA_PUBLIC_KEY \
-token 2 \
-amount 10
```

Проверим баланс командой \(баланс появится не сразу, нужно подождать около 5 минут\):

```text
coda client get-balance \
-token 2 \
-public-key $CODA_PUBLIC_KEY
```

Через некоторое время мы должны увидеть на балансе 1,000 coda токенов.

{% code title="\#ПРИМЕР ОТВЕТА" %}
```text
coda client get-balance \
-token 2 \
-public-key $CODA_PUBLIC_KEY
Balance: 1000 tokens
```
{% endcode %}

### 9. Отправка токенов

Теперь можно отправить токены.  
Чтобы это сделать нам сначала нужно добавить получателя командой ниже:

```text
coda client create-token-account \
-sender $CODA_PUBLIC_KEY \
-receiver B62qoDWfBZUxKpaoQCoFqr12wkaY84FrhxXNXzgBkMUi2Tz4K8kBDiv \
-token 2
```

Например отправим 50 токенов.   
В поле `-memo "My First TX"` вместо `My First TX` можно вписать что угодно. Либо оставить так, как есть.

```text
coda client send-payment \
-sender $CODA_PUBLIC_KEY \
-receiver B62qoDWfBZUxKpaoQCoFqr12wkaY84FrhxXNXzgBkMUi2Tz4K8kBDiv \
-token 2 \
-fee 0.1 \
-amount 50
```

Готово. Токены отправлены.

