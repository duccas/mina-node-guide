# Setting up Snark Stopper

## Description

The script will be useful for those who run the Block Producer and Snark Worker on the same server. Worker uses all your CPU cores 100%, which negatively affects the block producer. The script allows you to stop the Worker Snark 3 minutes before the block is produced and turn it on again after 10 minutes.

{% hint style="info" %}
This script was created by the user @whataday2day\#1271  
[https://github.com/c29r3/coda-snark-stopper](https://github.com/c29r3/coda-snark-stopper)
{% endhint %}

## Preparation

### Install JSON

```text
sudo apt install jq -y
```

### **Install Python 3.6**

Set the following requirements

```text
sudo apt-get install software-properties-common python-software-properties
```

Run the following command to add the PPA:

```text
sudo add-apt-repository ppa:jonathonf/python-3.6
```

Press Enter to continue.

Update repositories:

```text
sudo apt-get update
```

Finally, install Python 3.6:

```text
sudo apt-get install python3.6
```

After installation, you can check the installed version by running the following command:

```text
python3.6 -V
```

## Installing Snark Stopper

Install the git package if it is not installed on the server:

```text
yes | sudo apt install git
```

{% hint style="info" %}
Your snark worker should be RUNNING.
{% endhint %}

Check the config file. There are several options that you can remap.

### Install:

{% hint style="info" %}
You need to run in a new TMUX session
{% endhint %}

```text
git clone https://github.com/c29r3/coda-snark-stopper.git \
&& cd coda-snark-stopper \
&& pip3 install -r requirements.txt \
&& python3 snark-stopper.py
```

Now you need to add your public key and Worker fee to the stopper config. Open the config with the command:

```text
nano $HOME/coda-snark-stopper/config.yml
```

In the `WORKER_PUB_KEY: YOUR_PUBLIC_KEY` line, change `YOUR_PUBLIC_KEY` to `$CODA_PUBLIC_KEY` In the line `WORKER_FEE: 1`, replace the commission value, for example, from 1 to 0.25 

Done.

