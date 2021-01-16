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
sudo docker pull gcr.io/o1labs-192920/coda-daemon-baked:0.2.6-5c08d6d-5c08d6d-testworld-2258826
```

Where `gcr.io/o1labs-192920/coda-daemon-baked:0.2.6-5c08d6d-5c08d6d-testworld-2258826` - version of the new docker image.

## 3. Launch

Everything is ready to launch.   
Now select the required item to start from section \#2 by the link below:

{% page-ref page="node-launch-options/running-mina-with-docker.md" %}

## 4. Optional

If you need to clear the config folder, run the command below:

```text
rm -rf $HOME/.coda-config
```

