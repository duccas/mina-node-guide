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

In the `WORKER_PUB_KEY: YOUR_PUBLIC_KEY` line, change `YOUR_PUBLIC_KEY` to `$MINA_PUBLIC_KEY` In the line `WORKER_FEE: 1`, replace the commission value, for example, from 1 to 0.025 \(1000000000 to 25000000\)

{% hint style="info" %}
1 MINA = 1,000,000,000 nanomina
{% endhint %}

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

In the `WORKER_PUB_KEY: YOUR_PUBLIC_KEY` line, change `YOUR_PUBLIC_KEY` to `$MINA_PUBLIC_KEY` In the line `WORKER_FEE: 1`, replace the commission value, for example, from 1 to 0.025 \(1000000000 to 25000000\)

{% hint style="info" %}
1 MINA = 1,000,000,000 nanomina
{% endhint %}

Run container:

```text
docker run -d \
--volume $(pwd)/config.yml:/mina/config.yml \
--net=host \
--restart always \
--name snark-stopper \
c29r3/snark-stopper
```

Logs:

```text
docker logs -f snark-stopper
```

