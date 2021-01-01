# Setting up Snark Stopper

## Description

The script will be useful for those who run the Block Producer and Snark Worker on the same server. Worker uses all your CPU cores 100%, which negatively affects the block producer. The script allows you to stop the Worker Snark 3 minutes before the block is produced and turn it on again after 10 minutes.

{% hint style="info" %}
This script was created by the user @whataday2day\#1271  
[https://github.com/c29r3/mina-snark-stopper](https://github.com/c29r3/mina-snark-stopper)
{% endhint %}

## Preparation

### Install JSON

```text
sudo apt install jq -y
```

### Install the git package if it is not installed on the server:

```text
yes | sudo apt install git
```

{% hint style="info" %}
Your snark worker should be RUNNING.
{% endhint %}

Check the config file. There are several options that you can remap.

Unblock the private network:

```text
sudo iptables -D OUTPUT -p tcp -d 172.16.0.0/12 -j DROP
```

### 1. Install without Docker:

{% hint style="info" %}
You need to run in a new TMUX session
{% endhint %}

```text
sudo apt-get update \
&& sudo apt-get install python3-venv tmux git -y \
&& git clone https://github.com/c29r3/mina-snark-stopper.git \
&& cd mina-snark-stopper \
&& python3 -m venv venv \
&& source ./venv/bin/activate \
&& pip3 install -r requirements.txt
```

Now you need to add your public key and Worker fee to the stopper config. Open the config with the command:

```text
nano $HOME/mina-snark-stopper/config.yml
```

In the `WORKER_PUB_KEY: YOUR_PUBLIC_KEY` line, change `YOUR_PUBLIC_KEY` to `$MINA_PUBLIC_KEY` In the line `WORKER_FEE: 1`, replace the commission value, for example, from 1 to 0.025 

Done.

### 1.1 Run

```text
cd mina-snark-stopper; \
tmux new -s snark-stopper -d venv/bin/python3 snark-stopper.py
```

Logs:

```text
tmux attach -t snark-stopper
```

### 2. Install with Docker:

Download the config file:

```text
wget https://raw.githubusercontent.com/c29r3/mina-snark-stopper/master/config.yml
```

Now you need to add your public key and Worker commission to the stopper config.   
Open the config with the command:

```text
nano $HOME/config.yml
```

In the `WORKER_PUB_KEY: YOUR_PUBLIC_KEY` line, change `YOUR_PUBLIC_KEY` to `$MINA_PUBLIC_KEY` In the line `WORKER_FEE: 1`, replace the commission value, for example, from 1 to 0.025 

Run container:

```text
sudo docker run -d \
--volume $(pwd)/config.yml:/mina/config.yml \
--net=host \
--restart always \
--name snark-stopper \
c29r3/snark-stopper
```

Logs:

```text
sudo docker logs -f snark-stopper
```

### 3. Troubleshooting

If the snark-stopper can't connect to port `3085`:

Check port availability:

```text
nc -t -vv localhost 3085
```

Output should be something like this:  
`Connection to localhost 3085 port [tcp/*] succeeded!`

If the connection hangs, then the following options are possible:

* Access to port `3085` is blocked via ufw\iptables
* You did not add a docker container flag `-p 127.0.0.1:3085:3085`
* Node is not synced yet. For this reason the stopper can't connect

Port responds, but the stopper still can't connect:

```text
iptables -D OUTPUT -p tcp -d 172.16.0.0/12 -j DROP
```

it's because of the blocking of private subnets that the docker uses.

**Update docker image**

After running the command below, go to step 2

```text
sudo docker rm -f snark-stopper \
&& sudo docker pull c29r3/snark-stopper
```

### 4. How to Update

Delete the config file and container and download the new image:

```text
rm config.yml \
&& sudo docker rm -f snark-stopper \
&& sudo docker pull c29r3/snark-stopper
```

Then we continue from point 1 or 2.

### 5. Uninstall

```text
rm -rf mina-snark-stopper; \
sudo docker rm -f snark-stopper; \
sudo docker system prune -af
```

