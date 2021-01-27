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
sudo apt-get install -y curl unzip mina-testnet-postake-medium-curves=0.2.9-a940247
```

Where `mina-testnet-postake-medium-curves=0.2.9-a940247` - version of the new package.

## 3. Change configuration

{% hint style="info" %}
Do this item only if you need to change something in the configuration files. `.mina-env` or service file`mina.service .`

For example, in the latest update, you need to add the flag `-super-catchup`
{% endhint %}

Go to the file with flags:

```text
nano .mina-env
```

Add a flag `-super-catchup` to the end of the line `EXTRA_FLAGS`

```text
EXTRA_FLAGS=" -file-log-level Debug -super-catchup"
```

Save and exit: CTRL+S and CTRL+X

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

