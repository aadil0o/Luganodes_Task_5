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


      

