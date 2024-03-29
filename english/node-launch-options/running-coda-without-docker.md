# Running Mina without Docker

{% hint style="warning" %}
We recommend using `tmux`before starting to run multiple sessions in one terminal.
{% endhint %}

{% content-ref url="../setting-up-tmux.md" %}
[setting-up-tmux.md](../setting-up-tmux.md)
{% endcontent-ref %}

## 1. Firewall configuration

Open ports 22, 8302 and 8303 and activate the Firewall:

```
sudo ufw allow 22 \
&& sudo ufw allow 8302 \
&& sudo ufw allow 8303 \
&& yes | sudo ufw enable
```

We check the status of open ports with the command:

```
sudo ufw status
```

{% hint style="info" %}
If you do not have UFW installed on your server, install it using the command `sudo apt install ufw`
{% endhint %}

## 2. Installation for macOS

Need to install [Homebrew](https://brew.sh/).

```
brew install wget
```

Installing packages `coda`.

```
brew install minaprotocol/mina/mina
```

Running a node:

```
brew services start mina
```

If you already have the `mina` package installed, you need to update it with the command below. If you have not previously installed `mina`, then you can skip this command.

```
brew upgrade mina
```

## 3. Installation for `Ubuntu 18.04 / Debian 9`

Let's create a folder `.coda-config`:

```
mkdir .mina-config
```

Downloading package `Mina`:

```
echo "deb [trusted=yes] http://packages.o1test.net $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/mina.list
sudo apt-get update
sudo apt-get install -y curl unzip mina-mainnet=1.3.0-9b0369c
```

## 4. Launch options

### 4.1 Launch in Service

Setting up the `mina-env` file:

```
nano .mina-env
```

We copy and paste the variables into the file after entering your password from the key instead of `YOUR PASS FOR KEYS and KEYPATH`:

```
MINA_PRIVKEY_PASS="YOUR PASS FOR KEYS"
LOG_LEVEL=Info
FILE_LOG_LEVEL=Debug
EXTRA_FLAGS=" --block-producer-key $KEYPATH"
PEER_LIST_URL=https://storage.googleapis.com/mina-seed-lists/mainnet_seeds.txt
```

Save ans exit: CTRL+S and CTRL+X

### 4.1.1 Adding Snark Worker flags (if needed)

Add to file `.mina-env` Snark worker flags with your key and fee:

```
EXTRA_FLAGS=" -snark-worker-fee 0.025 -run-snark-worker B62qkWFkU9PDSzAxWWXVcxxHe1nJnfGqLeYbtxDLv5BxPiekGcxLTpj -work-selection seq -file-log-level Debug "
```

By default, the `-work-selection` for a snark worker is random `rand`. You can change this by adding the `-work-selection seq` flag to the command, which will work on jobs in the order required to be included from the scan state and will likely result in your snarks being included without a potentially lengthy delay.

### 4.1.2 Start the service

```
systemctl --user daemon-reload
systemctl --user start mina
systemctl --user enable mina
sudo loginctl enable-linger
```

Viewing logs:

```
journalctl --user-unit mina -n 1000 -f
```

### 4.2 Running a node in TMUX

Start an empty session in Tmux:

```
tmux new -s session
```

More about TMUX:

{% content-ref url="../setting-up-tmux.md" %}
[setting-up-tmux.md](../setting-up-tmux.md)
{% endcontent-ref %}

And launch in session with the command:

```
mina daemon \
--peer-list-url https://storage.googleapis.com/mina-seed-lists/mainnet_seeds.txt \
--generate-genesis-proof true
```

Before starting the block producer, you need to import and unlock the keys:

```
mina accounts import -privkey-path $KEYPATH
mina accounts unlock -public-key $MINA_PUBLIC_KEY
```

Run Block Producer:

```
mina client set-staking -public-key $MINA_PUBLIC_KEY
```

Run Snark Worker:

```
mina client set-snark-work-fee 0.025
mina client set-snark-worker -address $MINA_PUBLIC_KEY
```

Here you can set the Worker commission `coda client set-snark-work-fee 0.25`, or leave it as it is.&#x20;

Next, go to the next section and start with Point 2:

{% content-ref url="../cli.-key-import-sending-tokens.md" %}
[cli.-key-import-sending-tokens.md](../cli.-key-import-sending-tokens.md)
{% endcontent-ref %}
