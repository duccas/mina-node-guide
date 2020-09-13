# Running Coda with Docker

## 1. Node launch options

{% hint style="warning" %}
IMPORTANT! Read each point of the guide carefully so that there are no problems with the launch.
{% endhint %}

### Launch of the Block Producer:

Variables description:

`--name coda` - you can use any name for the container, or leave it as it is

```text
sudo docker run -d \
-e "CODA_PRIVKEY_PASS=$CODA_PASS" \
--mount type=bind,source="$(pwd)"/keys,target=/root/keys \
--name coda \
-p 8302:8302 \
-p 8303:8303 \
--restart always \
codaprotocol/coda-daemon:0.0.12-beta-feature-bump-genesis-timestamp-3e9b174 daemon \
-block-producer-key $HOME/keys/my-wallet \
-peer $SEED1 \
-peer $SEED2
```

### Snark Worker Launch:

{% hint style="warning" %}
If you don't want to launch the Snark Worker. You can go directly to step 3.
{% endhint %}

Before starting Worker, you need to configure the Firewall:

```text
sudo ufw allow 8305
```

Variables description:

1. `--name coda-worker` - you can use any name for the container, or leave it as it is
2. `--memory 16g` - limiting the amount of RAM that the container can use
3. `--cpus 8` - limiting the number of processor cores that a container can use
4. `-snark-worker-fee 0.25` - you can set the Snark Worker fee

You need to enter all the data in the command below:

```text
sudo docker run -d \
--name coda-worker \
-p 8305:8305 \
--memory 16g \
--cpus 8 \
--restart always \
codaprotocol/coda-daemon:0.0.12-beta-feature-bump-genesis-timestamp-3e9b174 daemon \
-run-snark-worker $CODA_PUBLIC_KEY \
-snark-worker-fee 0.25 \
-work-selection seq \
-peer $SEED1 \
-peer $SEED2 \
-external-port 8305
```

## 2. Viewing logs

View running containers:

```text
sudo docker ps -a
```

Block Producer container logs:

```text
sudo docker logs --follow coda -f
```

Snark Worker container logs:

```text
sudo docker logs --follow coda-worker -f
```

### Alternative log output

```text
sudo docker exec coda coda client status | grep "Block producers"
```

The output will only show the line with the block producer running. See example below:

{% code title="\#EXAMPLE" %}
```text
root@Coda:~# sudo docker exec coda coda client status | grep "Block producers"
Block producers running:         1 (4vsRCVfshM6QYPWn8TFMLdYbCdf9abRW1t71dAjCXQPYURMmxVPFe4VjXfrxjYeFWEzMmqTpc8suhsRvA51NjvRe6rmWv9eerUjRJFjdRTWcoBdyuyDnGC3GbtKdWhv5b9CajERMD7PHj3z4)
```
{% endcode %}

![](../../.gitbook/assets/image.png)

## 3. Docker commands

The container is stopped by the command:

```text
sudo docker stop coda
```

```text
sudo docker stop coda-worker
```

Removing a container:

```text
sudo docker rm coda
```

```text
sudo docker rm coda-worker
```

Removing a running container:

```text
sudo docker rm -f coda
```

```text
sudo docker rm -f coda-worker
```

