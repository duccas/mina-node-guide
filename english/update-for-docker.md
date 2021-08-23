---
description: Simple steps to update a node.
---

# Update for Docker

## 1. Deleting

Stopping and removing container:

```text
sudo docker rm -f mina
```

## 2. Settings

Removing all stopped containers and unused images:

```text
sudo docker system prune -a
```

Downloading the new update:

```text
sudo docker pull minaprotocol/mina-daemon-baked:1.1.5-a42bdee
```

Where `1.1.5` - version of the new docker image.

Download new file with peers:

```text
wget -O ~/peers.txt https://storage.googleapis.com/mina-seed-lists/mainnet_seeds.txt
```

## 3. Launch

Everything is ready to launch.   
Now select the required item to start from section \#2 by the link below:

{% page-ref page="node-launch-options/running-mina-with-docker.md" %}

## 4. Optional

If you need to clear the config folder, run the command below:

```text
rm -rf $HOME/.mina-config
```

