# Guide to Process Files with URLs

This guide is intended for Mac OS.
The process can be divided into 3 steps:

- Setup and configure environment to execute scripts
- Process initial file in multi threads to find working https connections
- Process result from previous step to find working TLS certificates

## Step 1 - Configure environment

Open the terminal and install Rust using rustup

```sh
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

Choose option 1) Proceed with installation (default)

After installation is completed run:

```sh
source $HOME/.cargo/env
```

## Step 2 - Get intermidiate result

Create a directory to contain the required project files:

```sh
mkdir $HOME/TLS && cd $HOME/TLS
```

Install git:

```sh
brew install git
```

Clone the repo:

```sh
git clone https://github.com/omxx/fhc.git
```

Move the folder $HOME/TLS/fhc and build the binary:

```sh
cd $HOME/TLS/fhc && cargo build --release
```

Place the file with URLs into the created folder $HOME/TLS under name hosts.txt
Check if the destination is correct, you will see some file content:
Hint: press q to quit

```sh
less $HOME/TLS/hosts.txt
```

Create a folder for the output results:

```sh
mkdir $HOME/TLS/results
```

Make sure you are in the correct folder:

```sh
cd $HOME/TLS/fhc
```

Execute the following command to get intermediate results:
Note that this may be a time consuming with 100K file (20 min approx)

```sh
cat ../hosts.txt | ./target/release/fhc -2 -t 1000 | grep https >> ../results/medium.txt
```

Resulting file is under $HOME/TLS/results/medium.txt with URLs that have TLS connection:

```sh
cat $HOME/TLS/results/medium.txt
```

## Step 3 - Get final result

Move the working folder $HOME/TLS

```sh
cd $HOME/TLS
```

Clone another repo:

```sh
git clone https://github.com/omxx/rustls.git
```

Move the folder $HOME/TLS/rustls and build the binary:

```sh
cd $HOME/TLS/rustls && cargo build --release
```

Make script executable:

```sh
chmod +x check.sh
```

Run the script:

```sh
./check.sh
```

You will find the resulting file under $HOME/TLS/results/final.txt

```sh
cat $HOME/TLS/results/final.txt
```

