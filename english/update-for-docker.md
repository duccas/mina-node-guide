---
description: Simple steps to update a node.
---

# Update for Docker

## 1. Deleting

Stopping and removing container:

```
sudo docker rm -f mina
```

## 2. Settings

Removing all stopped containers and unused images:

```
sudo docker system prune -a
```

Downloading the new update:

```
sudo docker pull minaprotocol/mina-daemon:1.3.1.1-f361ba1-bionic-mainnet
```

## 3. Launch

Everything is ready to launch. \
Now select the required item to start from section #2 by the link below:

{% content-ref url="node-launch-options/running-mina-with-docker.md" %}
[running-mina-with-docker.md](node-launch-options/running-mina-with-docker.md)
{% endcontent-ref %}

## 4. Optional

If you need to clear the config folder, run the command below:

```
rm -rf $HOME/.mina-config
```
