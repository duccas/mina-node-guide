# Running Coda without Docker

{% hint style="warning" %}
We recommend using `tmux`before starting to run multiple sessions in one terminal.
{% endhint %}

## 1. Installation for macOS

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

## 2. Installation for `Ubuntu 18.04 / Debian 9`

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

### 2.1 Launch of the Block Producer:

```text
coda daemon \
-peer $SEED1 \
-block-producer-pubkey $CODA_PUBLIC_KEY
```

### 2.2 Snark Worker Launch:

Variables description:

`-snark-worker-fee 0.25` - you need to set the worker's fee, or leave it as it is

```text
coda daemon \
-peer $SEED1 \
-run-snark-worker $CODA_PUBLIC_KEY \
-snark-worker-fee 0.25 
```

