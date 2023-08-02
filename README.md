##Cosmos Setup Guide


#Requirements

Operating System: Linux (Ubuntu, CentOS, or similar) or macOS (Windows may work, but Linux/macOS is recommended for development).
Processor: 64-bit Intel or AMD processor with support for VT-x/AMD-V virtualization.
Memory: At least 8GB RAM (16GB recommended).
Storage: At least 50GB of free disk space.
Internet Connection: A stable internet connection is necessary to download dependencies.

#Installation Steps

# 1. Install build tools and Go.
      `sudo apt-get update`
      `sudo apt-get install -y make gcc`
      `wget https://go.dev/dl/go1.20.3.linux-amd64.tar.gz`
      `sudo rm -rf /usr/local/go && sudo tar -C /usr/local -xzf go1.20.3.linux-amd64.tar.gz`
      `export PATH=$PATH:/usr/local/go/bin`

