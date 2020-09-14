# Preparing public keys

## 1. Preparing public keys

Before starting the node, you need to copy the folder with the keys to your server, created earlier for registration in the testnet 3.3.

{% hint style="info" %}
This step must be performed if, when creating public keys, you saved them to your local computer and deleted the server. If you did not delete the server and use it to start the node, then you should go to step 1.2.
{% endhint %}

Use any file manager \(Filezilla\) and connect to your server. Copy the `keys` folder to the server root.

![](../../.gitbook/assets/image%20%281%29.png)

### 1.2 You need to give the folders and files read and write permissions:

```text
chmod 700 $HOME/keys
chmod 600 $HOME/keys/my-wallet
```

## 2. Exporting keys and SEED peers

Now we will write your public key and SEED peers to the server in the `.profile` file so that they will not be exported next time \(peers should send them to your email before launching testnet 3.3\).

Replace `YOUR SEED1` and `YOUR SEED2` with the peers sent to you by email. And also `YOUR PASSWORD` for the public key password.

```text
echo 'export CODA_PUBLIC_KEY=$(cat $HOME/keys/my-wallet.pub)' >> $HOME/.profile
echo 'export SEED1=YOUR SEED1' >> $HOME/.profile
echo 'export SEED2=YOUR SEED2' >> $HOME/.profile
echo 'export CODA_PASS=YOUR PASSWORD' >> $HOME/.profile
source .profile
```

An example of how it should look:

{% code title="\#EXAMPLE" %}
```text
echo 'export CODA_PUBLIC_KEY=$(cat $HOME/keys/my-wallet.pub)' >> $HOME/.profile
echo 'export SEED1=/ip4/34.74.183.100/tcp/10001/ipfs/12D3KooWAFFq2yEQFFzhU5dt64AWqawRuomG9hL8rSmm5vxhAsgr' >> $HOME/.profile
echo 'export SEED2=/ip4/35.231.128.243/tcp/10001/ipfs/12D3KooWB79AmjiywL1kMGeKHizFNQE9naThM2ooHgwFcUzt6Yt1' >> $HOME/.profile
echo 'export CODA_PASS=qwerty123' >> $HOME/.profile
source .profile
```
{% endcode %}

