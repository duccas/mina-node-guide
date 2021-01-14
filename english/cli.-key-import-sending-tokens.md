# CLI. Key import, sending tokens

## Using CLI

### 1. To access [Mina CLI](https://minaprotocol.com/docs/cli-reference) commands :

{% hint style="warning" %}
This step only needs to be done if you started the node using Docker. If you haven't used Docker, skip straight to step 2.
{% endhint %}

```text
sudo docker exec -it mina bash
```

The CLI is exited with the command:

```text
exit
```

### 2. Checking status, node status:

```text
coda client status
```

It looks like this:

![](../.gitbook/assets/image%20%283%29.png)

Wait for the node to sync. The `Sync status:` field should say `Synced`. If the status says `Catched`, then you need to wait a little longer. After that you can start importing your keys.

### 3. Importing keys

 We import an account with a key with the following command:

```text
coda accounts import -privkey-path $HOME/keys/my-wallet
```

The list of your accounts can be viewed with the command below:

```text
coda accounts list
```

### 4. Account unlocking:

First, let's export the Public Key:

```text
export MINA_PUBLIC_KEY=$(cat $HOME/keys/my-wallet.pub)
```

Let's unlock the account so that you can move tokens:

```text
coda accounts unlock -public-key $MINA_PUBLIC_KEY
```

In the password input field, write your password from the key and press ENTER.

### 5. Transactions

Now you can send tokens.

You need to add:

* recipient `-receiver`
* transaction fee `-fee`
* the number of tokens sent `-amount`

```text
coda client send-payment \
-sender B62qnJHBeVJqWamtDhWDPwrX7Y5jiXKcMKTuvug9LQ8ictwNTWN7YvJ \
-receiver B62qndJi5mnRoBZ8SAYDM1oR2SgAk5WpZC8hGpJUZ4e64kDHGbFMeLJ \
-fee 0.1 \
-amount 2
```

Done. Tokens have been sent.

### 6. Check the balance of tokens

The token balance is checked by the command below with your token ID:

```text
coda client get-balance \
-public-key $MINA_PUBLIC_KEY
```

We will see the balance of mina tokens.

{% hint style="warning" %}
Attention!   
Points from 7 to 10 will not be used in testnet 5.1
{% endhint %}

### 7. Token creation

Now let's create tokens:

```text
coda client create-token \
-sender $MINA_PUBLIC_KEY
```

In response, we get the following:

{% code title="\#EXAMPLE OF ANSWER " %}
```text
Dispatched create new token command with ID 2cUDm3QoJ14znWj5LxN8hjwwuvtwi9FGXcy56i4dPvYizPNwr3V9c9ePYxy3pJKjgogNx28jwHhqupi6wHFgXBmU5iX27iK1zUvJarj6wJsUG8segWXc4LGPed66YbYk3u9HiWw4v8cYYEqcy1mU6hqfj5JPMPthEBifxUMHZTqCwZmYWSdiERxB6PtPEdXVraWaYPVU4Q8vtpSN7oSTK1AXyXLYYR835CBrNSmgbLvoBDNroCKwcQrzw4b76BFNLe6EuWvBcMgX6npeeAbPg8z8iJ4PKz3gA64o1Y72kCrqyqus718LwXcmp5jxsYvJB2CJHzyZ
```
{% endcode %}

### 8. Get the ID of the tokens

To carry out the following operations, we need to know the ID of the tokens. We get it with the following command:

```text
coda client get-tokens \
-public-key $MINA_PUBLIC_KEY
```

In response, we get the following:

{% code title="\#EXAMPLE OF ANSWER" %}
```text
Accounts are held for token IDs:
1
```
{% endcode %}

### 9. Mint tokens

To mince new tokens, you need to run the `mint-tokens` command. 1,000 tokens will be created in the account of the sender of the transaction under token ID 2.

```text
coda client mint-tokens \
-sender $MINA_PUBLIC_KEY \
-token 2 \
-amount 10
```

Let's check the balance with the command \(the balance will not appear immediately, you need to wait about 5 minutes\):

```text
coda client get-balance \
-token 2 \
-public-key $MINA_PUBLIC_KEY
```

After a while, we should see 1,000 coda tokens on our balance.

{% code title="\#EXAMPLE OF ANSWER" %}
```text
coda client get-balance \
-token 2 \
-public-key $MINA_PUBLIC_KEY
Balance: 1000 tokens
```
{% endcode %}

### 10. Sending tokens

Now you can send tokens.   
To do this, we first need to add a recipient with the command below:

```text
coda client create-token-account \
-sender $MINA_PUBLIC_KEY \
-receiver B62qoDWfBZUxKpaoQCoFqr12wkaY84FrhxXNXzgBkMUi2Tz4K8kBDiv \
-token 2
```

For example, let's send 50 tokens.   
In the `-memo "My First TX"` field, instead of `My First TX`, you can enter anything you want. Or leave it as it is.

```text
coda client send-payment \
-sender $MINA_PUBLIC_KEY \
-receiver B62qoDWfBZUxKpaoQCoFqr12wkaY84FrhxXNXzgBkMUi2Tz4K8kBDiv \
-token 2 \
-fee 0.1 \
-amount 50
```

Done. Tokens have been sent.

