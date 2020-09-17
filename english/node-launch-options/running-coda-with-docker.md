# Running Coda with Docker

## 1. Preparation

### 1.1 Ubuntu setup

Update the packages on the server to the latest versions:

```text
sudo apt update && sudo apt upgrade -y
```

Install Docker:

```text
sudo apt install docker.io curl -y
```

Activate Docker:

```text
sudo systemctl start docker
sudo systemctl enable docker
```

### 1.2 Firewall configuration

{% hint style="info" %}
If your VPS server has a built-in Firewall, then you should open ports 8302 and 8303 there.

If not, then follow the commands below:
{% endhint %}

Open ports 8302 and 8303:

```text
sudo iptables -A INPUT -p tcp --dport 8302 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 8303 -j ACCEPT
```

## 2. Node launch options

{% hint style="warning" %}
Choose only one launch option from the 2 suggested below \(paragraph 2.1 or 2.2\).
{% endhint %}

### 2.1 Run only Block Producer:

Variables description:

`--name coda` - you can use any name for the container, or leave it as it is

```text
sudo docker run -d \
-e "CODA_PRIVKEY_PASS=$CODA_PASS" \
--mount type=bind,source="$(pwd)"/keys,target=$HOME/keys \
--name coda \
-p 8302:8302 \
-p 8303:8303 \
--restart always \
codaprotocol/coda-daemon:0.0.16-beta6 daemon \
-block-producer-key $HOME/keys/my-wallet \
-peer $SEED1
```

### 2.2 Launch of Block Producer with Snark Worker:

{% hint style="warning" %}
If you don't want to launch the Snark Worker. You can go directly to step 3.
{% endhint %}

First you need to close port 3085:

```text
sudo ufw deny 3085
```

Variables description:

1. `--name coda` - you can use any name for the container, or leave it as it is
2. `--memory 16g` - limiting the amount of RAM that the container can use
3. `--cpus 8` - limiting the number of processor cores that a container can use
4. `-snark-worker-fee 0.25` - you can set the Snark Worker fee

You need to enter all the data in the command below:

```text
sudo docker run -d \
-e "CODA_PRIVKEY_PASS=$CODA_PASS" \
--mount type=bind,source="$(pwd)"/keys,target=$HOME/keys \
--name coda \
-p 8302:8302 \
-p 8303:8303 \
-p 127.0.0.1:3085:3085 \
--memory 16g \
--cpus 8 \
--restart always \
codaprotocol/coda-daemon:0.0.16-beta6 daemon \
-peer $SEED1 \
-block-producer-key $HOME/keys/my-wallet \
-run-snark-worker $CODA_PUBLIC_KEY \
-snark-worker-fee 0.25 \
-work-selection seq
```

{% hint style="info" %}
For both Block Producer and Snark Worker to work, you need to configure the Snark Stopper. To briefly stop the Worker during block production. 

Follow the link below to set up your Snark Stopper.
{% endhint %}

{% page-ref page="../setting-up-snark-stopper.md" %}

## 3. Viewing logs

View running containers:

```text
sudo docker ps -a
```

Container logs:

```text
sudo docker logs --follow coda -f
```

### 3.1 Alternative log output

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

## 4. Docker commands

The container is stopped by the command:

```text
sudo docker stop coda
```

Removing a container:

```text
sudo docker rm coda
```

Removing a running container:

```text
sudo docker rm -f coda
```

