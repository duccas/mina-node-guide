---
description: Simple steps to update a node in the Service.
---

# Update for Service

## 1. Stopping the Service

Stopping:

```text
systemctl --user stop mina
```

## 2. Settings

Removing the previous update:

```text
sudo apt-get remove mina-testnet-postake-medium-curves
```

Downloading the new update:

```text
echo "deb [trusted=yes] http://packages.o1test.net release main" | sudo tee /etc/apt/sources.list.d/mina.list
sudo apt-get update
sudo apt-get install -y curl unzip mina-mainnet=1.1.7-d5ff5aa
```

Where `1.1.7` - version of the new package.

Download new file with peers:

```text
wget -O ~/peers.txt https://storage.googleapis.com/mina-seed-lists/mainnet_seeds.txt
```

## 3. Change configuration

{% hint style="info" %}
Do this item only if you need to change something in the configuration files. `.mina-env` or service file`mina.service .`
{% endhint %}

Go to the file with flags:

```text
nano .mina-env
```

Changing data then save and exit: CTRL+S and CTRL+X

## 4. Launch

Reload the configuration change:

```text
systemctl --user daemon-reload
```

Start the Service:

```text
systemctl --user restart mina
```

Viewing logs:

```text
journalctl --user-unit mina -n 1000 -f
```

