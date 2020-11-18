# Preparing public keys

## 1. Preparing public keys

Before starting the node, you need to copy the folder with the keys to your server, created earlier for registration in the testnet 4.1.

{% hint style="info" %}
This step must be performed if, when creating public keys, you saved them to your local computer and deleted the server. If you did not delete the server and use it to start the node, then you should go to step 1.2.
{% endhint %}

Use any file manager \(Filezilla\) and connect to your server. Copy the `keys` folder to the server root.

![](../../.gitbook/assets/image%20%281%29.png)

### 1.2 You need to give the folders and files read and write permissions:

```text
chmod 700 $HOME/keys
```

## 2. Exporting keys

Now we will write your public key to the server in the `.profile` file so that they will not be exported next time \(peers should send them to your email before launching testnet 4.1\).

Replace `YOUR PASSWORD` for the public key password.

```text
echo 'export KEYPATH=$HOME/keys/my-wallet' >> $HOME/.profile
echo 'export MINA_PUBLIC_KEY=$(cat $HOME/keys/my-wallet.pub)' >> $HOME/.profile
echo 'export CODA_PRIVKEY_PASS=YOUR PASSWORD' >> $HOME/.profile
source ~/.profile
```

An example of how it should look:

{% code title="\#EXAMPLE" %}
```text
echo 'export KEYPATH=$HOME/keys/my-wallet' >> $HOME/.profile
echo 'export MINA_PUBLIC_KEY=$(cat $HOME/keys/my-wallet.pub)' >> $HOME/.profile
echo 'export CODA_PRIVKEY_PASS=qwerty123' >> $HOME/.profile
source ~/.profile
```
{% endcode %}

