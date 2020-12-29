# Preparing public keys and peers

## 1. Preparing public keys

Before starting the node, you need to copy the keys to your server. 

Let's delete the old key folder and create a new one:

```text
rm -rf ~/keys
mkdir ~/keys
```

The link to download the archive with the keys will come to your email. It looks like this: https://storage.googleapis.com/testworldkeys/852e16aab6622a2278d3ad08bd404a0f\_1937.zip

Replace`<YOUR LINK>` to the link from the email.

```text
wget -O ~/keys/new-keys.zip <YOUR LINK>
```

Example:

{% code title="\#ПРИМЕР" %}
```text
wget -O ~/keys/new-keys.zip https://storage.googleapis.com/testworldkeys/852e16aab6622a2278d3ad08bd404a0f_5454.zip
```
{% endcode %}

Let's go to the folder `keys` and unpack the downloaded files:

```text
cd ~/keys
unzip new-keys.zip
```

As a result, we will see that 2 files with the names `extra_fish_account_1937` and `extra_fish_account_1937.pub`.

![](../../.gitbook/assets/image%20%284%29.png)

Now these files need to be renamed to the correct view. Replace in commands below `<FILE NAME>` on file names after unpacking.

```text
mv <FILE NAME> my-wallet
mv <FILE NAME>.pub my-wallet.pub
```

Example:

{% code title="\#ПРИМЕР" %}
```text
mv extra_fish_account_1937 my-wallet
mv extra_fish_account_1937.pub my-wallet.pub
```
{% endcode %}

We paste into the terminal. Done.

### 1.2 You need to give the folders and files read and write permissions:

```text
chmod 700 $HOME/keys
chmod 600 $HOME/keys/my-wallet
```

## 2. Exporting keys

Now we will write your public key to the server in the `.profile` file so that they will not be exported next time \(peers should send them to your email before launching testnet 4.1\).

Replace `YOUR PASSWORD` for the public key password.

```text
echo 'export KEYPATH=$HOME/keys/my-wallet' >> $HOME/.bashrc
echo 'export MINA_PUBLIC_KEY=$(cat $HOME/keys/my-wallet.pub)' >> $HOME/.bashrc
echo 'export CODA_PRIVKEY_PASS=YOUR PASSWORD' >> $HOME/.bashrc
source ~/.bashrc
```

An example of how it should look:

{% code title="\#EXAMPLE" %}
```text
echo 'export KEYPATH=$HOME/keys/my-wallet' >> $HOME/.bashrc
echo 'export MINA_PUBLIC_KEY=$(cat $HOME/keys/my-wallet.pub)' >> $HOME/.bashrc
echo 'export CODA_PRIVKEY_PASS=qwerty123' >> $HOME/.bashrc
source ~/.bashrc
```
{% endcode %}

## 3. Preparation of peers

Before the launch of Testnet Testworld, participants should receive a link to download peers by email to launch the node.   
It looks something like this:

```text
wget -O ~/peers.txt https://raw.githubusercontent.com/MinaProtocol/coda-automation/bug-bounty-net/terraform/testnets/testworld/peers.txt
```

Insert into the terminal and download the peers.   
Done.

