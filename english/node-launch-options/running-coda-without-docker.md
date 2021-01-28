# Running Mina without Docker

{% hint style="warning" %}
We recommend using `tmux`before starting to run multiple sessions in one terminal.
{% endhint %}

{% page-ref page="../setting-up-tmux.md" %}

## 1. Firewall configuration

Open ports 22, 8302 and 8303 and activate the Firewall:

```text
sudo ufw allow 22 \
&& sudo ufw allow 8302 \
&& sudo ufw allow 8303 \
&& yes | sudo ufw enable
```

We check the status of open ports with the command:

```text
sudo ufw status
```

{% hint style="info" %}
If you do not have UFW installed on your server, install it using the command `sudo apt install ufw`
{% endhint %}

## 2. Installation for macOS

Need to install [Homebrew](https://brew.sh/).

```text
brew install wget
```

Installing packages `coda`.

```text
brew install minaprotocol/mina/mina
```

Running a node:

```text
brew services start mina
```

If you already have the `mina` package installed, you need to update it with the command below. If you have not previously installed `mina`, then you can skip this command.

```text
brew upgrade mina
```

## 3. Installation for `Ubuntu 18.04 / Debian 9`

Removing previous versions:

```text
sudo apt-get remove mina-testnet-postake-medium-curves
sudo apt-get remove mina-kademlia
```

Let's create a folder `.coda-config`:

```text
mkdir .coda-config
```

Downloading package `Mina`:

```text
sudo apt-get update
sudo apt-get install -y apt-transport-https ca-certificates
echo "deb [trusted=yes] http://packages.o1test.net release main" | sudo tee /etc/apt/sources.list.d/mina.list
sudo apt-get update
sudo apt-get install -y curl unzip mina-testnet-postake-medium-curves=0.2.11-d075f83
```

## 4. Launch options

### 4.1 Launch in Service

Setting up the `mina-env` file:

```text
nano .mina-env
```

We copy and paste the variables into the file after entering your password from the key instead of `YOUR PASS FOR KEYS`:

```text
CODA_PRIVKEY_PASS="YOUR PASS FOR KEYS"
EXTRA_FLAGS=" -file-log-level Debug -super-catchup "
```

Save ans exit: CTRL+S and CTRL+X

### 4.1.1 Adding Snark Worker flags \(if needed\)

Add to file `.mina-env` Snark worker flags with your key and fee:

```text
EXTRA_FLAGS=" -snark-worker-fee 0.025 -run-snark-worker B62qkWFkU9PDSzAxWWXVcxxHe1nJnfGqLeYbtxDLv5BxPiekGcxLTpj -work-selection seq -file-log-level Debug -super-catchup "
```

By default, the `-work-selection` for a snark worker is random `rand`. You can change this by adding the `-work-selection seq` flag to the command, which will work on jobs in the order required to be included from the scan state and will likely result in your snarks being included without a potentially lengthy delay.

### 4.1.2 Start the service

```text
systemctl --user daemon-reload
systemctl --user start mina
systemctl --user enable mina
sudo loginctl enable-linger
```

Viewing logs:

```text
journalctl --user-unit mina -n 1000 -f
```

### 4.2 Running a node in TMUX

Start an empty session in Tmux:

```text
tmux new -s session
```

More about TMUX:

{% page-ref page="../setting-up-tmux.md" %}

And launch in session with the command:

```text
coda daemon \
-peer-list-file ~/peers.txt \
-generate-genesis-proof true \
-log-level Info \
-super-catchup
```

Before starting the block producer, you need to import and unlock the keys:

```text
coda accounts import -privkey-path $KEYPATH
coda accounts unlock -public-key $MINA_PUBLIC_KEY
```

Run Block Producer:

```text
coda client set-staking -public-key $MINA_PUBLIC_KEY
```

Run Snark Worker:

```text
coda client set-snark-work-fee 0.025
coda client set-snark-worker -address $MINA_PUBLIC_KEY
```

Here you can set the Worker commission `coda client set-snark-work-fee 0.25`, or leave it as it is. 

Next, go to the next section and start with Point 2:

{% page-ref page="../cli.-key-import-sending-tokens.md" %}

