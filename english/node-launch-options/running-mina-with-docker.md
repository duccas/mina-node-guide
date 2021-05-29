# Running Mina with Docker

## 1. Preparation

### 1.1 Ubuntu setup

Update the packages on the server to the latest versions:

```text
sudo apt update && sudo apt upgrade -y
```

Install and activate Docker:

```text
sudo apt install docker.io curl -y \
&& sudo systemctl start docker \
&& sudo systemctl enable docker
```

### 1.2 Firewall configuration

{% hint style="info" %}
If your VPS server has a built-in Firewall, then you should open ports 8302 and 8303 there.

If not, then follow the commands below:
{% endhint %}

Open ports 8302 and 8303:

```text
sudo iptables -A INPUT -p tcp --dport 8302:8303 -j ACCEPT
```

## 2. Node launch options

{% hint style="warning" %}
Choose only one launch option from the 2 suggested below \(paragraph 2.1 or 2.2\).
{% endhint %}

### 2.1 Run only Block Producer:

Variables description:  
  
`--name mina` - you can use any name for the container, or leave it as it is;  
`-block-producer-password "YOUR PASS"` - instead `YOUR PASS` enter the password for your key.  
`$KEYPATH` - path to the file with the private key `my-wallet`. 

Optional:  
`--coinbase-receiver B62qp...` - flag to redirect block reward to another address.

```text
sudo docker run --name mina -d \
--restart always \
-p 8302:8302 \
-p 127.0.0.1:3085:3085 \
-v $(pwd)/keys:$HOME/keys:ro \
-v $(pwd)/.mina-config:$HOME/.mina-config \
minaprotocol/mina-daemon-baked:1.1.5-a42bdee daemon \
-block-producer-key $KEYPATH \
-block-producer-password "YOUR_PASS" \
--peer-list-url https://storage.googleapis.com/mina-seed-lists/mainnet_seeds.txt \
--insecure-rest-server \
--open-limited-graphql-port \
--limited-graphql-port 3095 \
--file-log-level Debug \
-log-level Info
```

### 2.1.1 Run Snark Worker to Block Producer:

{% hint style="warning" %}
If you don't want to launch Snark Worker. You can go directly to step 3.
{% endhint %}

Let's set the Worker fee:  
`set-snark-work-fee 0.025` - the commission value of `0.025` can be changed to any other.

```text
sudo docker exec -it mina mina client set-snark-work-fee 0.025
```

Run Worker:

```text
sudo docker exec -it mina mina client set-snark-worker -address $MINA_PUBLIC_KEY
```

{% hint style="info" %}
For both Block Producer and Snark Worker to work, you may configure the Snark Stopper. To briefly stop the Worker during block production. 

Follow the link below to set up your Snark Stopper.
{% endhint %}

{% page-ref page="../setting-up-snark-stopper.md" %}

### 2.2 Run only Snark Worker \(without Block Producer\)

First, let's launch a node without a Producer and a Snark.

Variables description:

`--name mina` - you can use any name for the container, or leave it as it is;

By default, the `-work-selection` for a snark worker is random `rand`. You can change this by adding the `-work-selection seq` flag to the command, which will work on jobs in the order required to be included from the scan state and will likely result in your snarks being included without a potentially lengthy delay;

`set-snark-work-fee 0.025` - the commission value of `0.025` can be changed to any other.

```text
sudo docker run --name mina -d \
--restart always \
-p 8302:8302 \
-p 127.0.0.1:3085:3085 \
-v $(pwd)/keys:$HOME/keys:ro \
-v $(pwd)/.mina-config:$HOME/.mina-config \
minaprotocol/mina-daemon-baked:1.1.5-a42bdee daemon \
--peer-list-url https://storage.googleapis.com/seed-lists/finalfinal3_seeds.txt \
-snark-worker-fee 0.025 \
-run-snark-worker $MINA_PUBLIC_KEY \
--insecure-rest-server \
--file-log-level Debug \
-log-level Info
```

## 3. Viewing logs

View running containers:

```text
sudo docker ps -a
```

Node container logs:

```text
sudo docker logs --follow mina -f --tail 1000
```

Node status:

```text
sudo docker exec -it mina mina client status
```

### 3.1 Alternative log output

```text
sudo docker exec -it mina mina client status | grep "Block producers"
```

The output will only show the line with the block producer running. See example below:

{% code title="\#EXAMPLE" %}
```text
root@Coda:~# sudo docker exec -it mina mina client status | grep "Block producers"
Block producers running:         1 (B62qpSphT9prqYrJFio82WmV3u29DkbzGprLAM3pZQM2ZEaiiBmyY82)
```
{% endcode %}

![](../../.gitbook/assets/image.png)

## 4. Docker commands

The container is stopped by the command:

```text
sudo docker stop mina
```

Restart container:

```text
sudo docker restart mina
```

Removing a container:

```text
sudo docker rm mina
```

Removing a running container:

```text
sudo docker rm -f mina
```

## 5. Others

Deleting config folder:

```text
rm -rf ~/.coda-config
```

