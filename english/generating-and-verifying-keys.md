# Generating and verifying keys

## 1. Installing

### Installing packages

```text
echo "deb [trusted=yes] http://packages.o1test.net release main" | sudo tee /etc/apt/sources.list.d/mina.list
sudo apt-get update
sudo apt-get install -y curl unzip mina-mainnet=1.1.5-a42bdee
```

### Installing the key generator

```text
sudo apt-get install mina-generate-keypair=0.2.12-718eba4
```

## 2. Generating keys

When creating keys, you will be asked to create a password.

### Option 1 with a package:

```text
mina-generate-keypair -privkey-path ~/keys/my-wallet
```

### Option 2 using docker:

```text
docker run  --interactive --tty --rm --volume $(pwd)/keys:/keys minaprotocol/generate-keypair:0.2.12-718eba4 -privkey-path /keys/my-wallet
```

### Set the rights:

```text
chmod 700 $HOME/keys
chmod 600 $HOME/keys/my-wallet
```

## 3. Key verification

When checking the key, you will be asked to enter the password from it.

### Option 1 with a package:

```text
mina-validate-keypair -privkey-path ~/keys/my-wallet
```

### Option 2 using docker:

```text
docker run --interactive --tty --rm --entrypoint=mina-validate-keypair --volume $(pwd)/keys:/keys minaprotocol/generate-keypair:0.2.12-718eba4 -privkey-path /keys/my-wallet
```

In both cases, if everything is ok with your keys, you will receive a message:

```text
Verified a transaction using specified keypair
```

It means your keys have been verified.

## 4. Exporting keys

Now let's write your public key to the server in the `.bashrc` file so that we don't export them again next time.

```text
echo 'export KEYPATH=$HOME/keys/my-wallet' >> $HOME/.bashrc
echo 'export MINA_PUBLIC_KEY=$(cat $HOME/keys/my-wallet.pub)' >> $HOME/.bashrc
source ~/.bashrc
```

