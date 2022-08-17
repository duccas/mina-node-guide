---
description: Simple steps to update a node in the Service.
---

# Update for Service

## 1. Stopping the Service

Stopping:

```
systemctl --user stop mina
```

## 2. Settings

Downloading the new update:

```
echo "deb [trusted=yes] http://packages.o1test.net $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/mina.list
sudo apt-get update
sudo apt-get install -y curl unzip mina-mainnet=1.3.0-9b0369c
```

## 3. Change configuration

{% hint style="info" %}
Do this item only if you need to change something in the configuration files. `.mina-env` or service file`mina.service .`
{% endhint %}

Go to the file with flags:

```
nano .mina-env
```

Changing data then save and exit: CTRL+S and CTRL+X

## 4. Launch

Reload the configuration change:

```
systemctl --user daemon-reload
```

Start the Service:

```
systemctl --user restart mina
```

Viewing logs:

```
journalctl --user-unit mina -n 1000 -f
```
