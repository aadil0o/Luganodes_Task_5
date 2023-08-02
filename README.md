## Cosmos Setup Guide


# Requirements

Operating System: Linux (Ubuntu, CentOS, or similar) or macOS (Windows may work, but Linux/macOS is recommended for development).
Processor: 64-bit Intel or AMD processor with support for VT-x/AMD-V virtualization.
Memory: At least 8GB RAM (16GB recommended).
Storage: At least 50GB of free disk space.
Internet Connection: A stable internet connection is necessary to download dependencies.

# Installation Steps

# 1. Install build tools and Go.
      sudo apt-get update
      sudo apt-get install -y make gcc
      wget https://go.dev/dl/go1.20.3.linux-amd64.tar.gz
      sudo rm -rf /usr/local/go && sudo tar -C /usr/local -xzf go1.20.3.linux-amd64.tar.gz
      export PATH=$PATH:/usr/local/go/bin
# 2. Build the gaiad binary and initialize the chain home folder.

      cd $HOME
      git clone https://github.com/cosmos/gaia
      cd gaia
      make install
      export PATH=$PATH:$HOME/go/bin
      gaiad init <custom_moniker>

custom_moniker = user-defined name or identifier for your node

# 3. Prepare the gensis file

      cd $HOME
      wget https://github.com/cosmos/testnets/raw/master/public/genesis.json.gz
      gzip -d genesis.json.gz
      mv genesis.json $HOME/.gaia/config/genesis.json
      
      # Set minimum gas price & peers
      cd $HOME/.gaia/config
      sed -i 's/minimum-gas-prices = ""/minimum-gas-prices = "0.0025uatom"/' app.toml
      sed -i 's/seeds = ""/seeds = "639d50339d7045436c756a042906b9a69970913f@seed-01.theta-testnet.polypore.xyz:26656,3e506472683ceb7ed75c1578d092c79785c27857@seed-02.theta-testnet.polypore.xyz:26656"/' config.toml

# 4. State setup

      
•Visit a testnet explorer (https://explorer.theta-testnet.polypore.xyz/)to find the block and hash for the current height - 1000.

•Set these parameters in the code snippet below: <BLOCK_HEIGHT> and <BLOCK_HASH>.

      cd $HOME/.gaia/config
      sed -i 's/enable = false/enable = true/' config.toml
      sed -i 's/trust_height = 0/trust_height = <BLOCK_HEIGHT>/' config.toml
      sed -i 's/trust_hash = ""/trust_hash = "<BLOCK_HASH>"/' config.toml
      sed -i 's/rpc_servers = ""/rpc_servers = "http:\/\/state-sync-01.theta-testnet.polypore.xyz:26657,http:\/\/state-sync-02.theta-testnet.polypore.xyz:26657"/' config.toml


# 5.Cosmovisor Setup

      go install github.com/cosmos/cosmos-sdk/cosmovisor/cmd/cosmovisor@v1.3.0
      mkdir -p ~/.gaia/cosmovisor/genesis/bin
      cp ~/go/bin/gaiad ~/.gaia/cosmovisor/genesis/bin/

# 6      Create Service File

      nano /etc/systemd/system/cosmovisor.service

Then save this file given below

      [Unit]
      Description=Cosmovisor service
      After=network-online.target
      
      [Service]
      User=root
      ExecStart=/root/go/bin/cosmovisor run start --x-crisis-skip-assert-invariants --home /root/.gaia
      Restart=no
      LimitNOFILE=4096
      Environment='DAEMON_NAME=gaiad'
      Environment='DAEMON_HOME=/root/.gaia'
      Environment='DAEMON_ALLOW_DOWNLOAD_BINARIES=true'
      Environment='DAEMON_RESTART_AFTER_UPGRADE=true'
      Environment='DAEMON_LOG_BUFFER_SIZE=512'
      Environment='UNSAFE_SKIP_BACKUP=true'
      
      [Install]
      WantedBy=multi-user.target


