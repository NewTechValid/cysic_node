# *Cysic* validator node



### **Step 1: Ensure System Requirements**
- **CPU:** 8+ cores
- **RAM:** 16+ GB (32 GB or more recommended for high performance)
- **Disk:** 500 GB SSD (preferably NVMe for faster performance)
- **Operating System:** Ubuntu 20.04 or 22.04 LTS
- **Network:** Stable internet connection with at least 1 Gbps bandwidth

---

### **Step 2: Install Dependencies**
Install essential dependencies for building blockchain nodes and working with Rust-based projects (since most ZK-proof-based platforms use Rust).

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install curl wget git build-essential libssl-dev pkg-config libclang-dev cmake -y
```

---

### **Step 3: Install Rust**
Since **Cysic** is likely built in Rust (a common choice for ZK-proof platforms), you will need to install the Rust programming language. To do so:

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
source $HOME/.cargo/env
rustc --version
```

This will install the Rust toolchain and ensure that it's available globally.

---

### **Step 4: Clone the Cysic Repository**
Once the Cysic project is live and the repository is released, you can clone it to start building the node. At this point, the command might look like:

```bash
git clone https://github.com/cysic/cysic-node.git
cd cysic-node
```

Ensure you’re on the correct branch or tag for the testnet release.

---

### **Step 5: Build the Cysic Node**
After cloning the repository, you’ll need to build the node. The typical build steps for a Rust project are as follows:

```bash
cargo build --release
```

This command will build the Cysic node in release mode. It might take a few minutes, depending on your system's performance.

---

### **Step 6: Initialize the Node**
Once the node is built, you need to initialize it with configuration specific to the Cysic testnet. The `init` command usually sets up directories and configuration files:

```bash
./target/release/cysic-node init --chain cysic-testnet --name <your_node_name>
```

This will create the necessary configuration files in your home directory (`~/.cysic` or a similar directory), such as `config.toml`, `genesis.json`, and `node_key.json`.

---

### **Step 7: Set Up Peers and Network Configuration**
To connect your node to the network, you need to set up peers. You can find a list of available peers from the Cysic community or official channels once the testnet is live.

Open the configuration file (`~/.cysic/config/config.toml` or the file created during initialization) and add or update the `persistent_peers` section with the peer list:

```bash
PEERS="<peer_list>"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$PEERS\"/" ~/.cysic/config/config.toml
```

Ensure that other parameters such as gas fees, block pruning, and networking settings are optimized for the testnet.

---

### **Step 8: Start the Node**
Once everything is configured, start the node using the following command:

```bash
./target/release/cysic-node start
```

You can monitor the node's logs to check if it’s syncing correctly and is properly connected to the testnet:

```bash
tail -f ~/.cysic/data/cysic.log
```

---

### **Step 9: Create a Validator**
Once your node is synced with the testnet, you can register as a validator. The process typically involves generating validator keys and submitting a validator registration transaction.

#### **Generate Validator Keys**
To generate the necessary keys for your validator (public and private keys), you can run:

```bash
./target/release/cysic-node key generate --validator
```

This will create your validator key pair.

#### **Submit Validator Registration**
After generating the keys, you can register your validator. Use the following command to submit a transaction to the network:

```bash
./target/release/cysic-node tx staking create-validator \
  --amount=1000000cysic \
  --pubkey=$(./target/release/cysic-node tendermint show-validator) \
  --moniker="<your_node_name>" \
  --chain-id=cysic-testnet \
  --commission-rate="0.10" \
  --commission-max-rate="0.20" \
  --commission-max-change-rate="0.01" \
  --min-self-delegation="1" \
  --from=<YOUR_WALLET> \
  --gas="auto" \
  --fees="500cysic"
```

Make sure to replace placeholders like `<your_node_name>` and `<YOUR_WALLET>` with your actual details.

---

### **Step 10: Monitor Your Node and Validator**
Once you’ve registered your validator, you can check the status of both your node and validator:

#### **Node Status:**

```bash
./target/release/cysic-node status
```

#### **Validator Status:**

To query the validator’s status:

```bash
./target/release/cysic-node query staking validator $(./target/release/cysic-node keys show <YOUR_WALLET> --bech val -a)
```

#### **Monitor Logs:**

You can also monitor the logs for any issues related to node synchronization or validation:

```bash
tail -f ~/.cysic/data/cysic.log
```
