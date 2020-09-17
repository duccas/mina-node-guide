# Running Coda without Docker

{% hint style="warning" %}
We recommend using `tmux`before starting to run multiple sessions in one terminal.
{% endhint %}

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
brew install codaprotocol/coda/coda
```

Running a node:

```text
brew services start coda
```

If you already have the `coda` package installed, you need to update it with the command below. If you have not previously installed `coda`, then you can skip this command.

```text
brew upgrade coda
```

## 3. Installation for `Ubuntu 18.04 / Debian 9`

Removing previous versions:

```text
sudo apt-get remove coda-testnet-postake-medium-curves
sudo apt-get remove coda-kademlia
```

Download the distribution `Coda`:

```text
echo "deb [trusted=yes] http://packages.o1test.net release main" | sudo tee /etc/apt/sources.list.d/coda.list
sudo apt-get update
sudo apt-get install coda-testnet-postake-medium-curves
```

### 3.1 Running a node

By the command:

```text
coda daemon \
-peer $SEED1
```

Block Producer Launch:

```text
coda client set-staking -public-key $CODA_PUBLIC_KEY
```

Launching Snark Worker:

```text
coda client set-snark-work-fee 0.25
coda client set-snark-worker -address $CODA_PUBLIC_KEY
```

Here you can set the Worker commission `coda client set-snark-work-fee 0.25`, or leave it as it is. 

Next, go to the next section and start with Point 2:

{% page-ref page="../cli.-key-import-sending-tokens.md" %}

## 4. Alternative launch of Block Producer and Snark Worker.

### 4.1 Launch of the Block Producer:

```text
coda daemon \
-peer $SEED1 \
-block-producer-pubkey $CODA_PUBLIC_KEY
```

### 4.2 Snark Worker Launch:

Variables description:

`-snark-worker-fee 0.25` - you need to set the worker's fee, or leave it as it is

```text
coda daemon \
-peer $SEED1 \
-run-snark-worker $CODA_PUBLIC_KEY \
-snark-worker-fee 0.25 
```

