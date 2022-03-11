# blockchain-access-layer-bitcoin-plugin

**Notice:** the build requires a library (btcd-cli4j) to communicate with the Bitcoin Core
node. [The used library](https://github.com/pythonmax/btcd-cli4j) is forked from
an [unmaintained library](http://btcd-cli4j.neemre.com) to fix some issues resulting from changes in the recent versions
of the Bitcoin Core node. However, the used library is not available in a public Maven repository, so we had to provide
a local Maven repository which includes the required binaries. This repository is found [here](local-maven-repo).

### Running a Local Bitcoin Core Node

A Bitcoin Core node (or _bitcoind_ node) is used to access the Bitcoin network. For development purposes, it is advised
not to connect to the main Bitcoin network, but rather to one of the testnets.
(another, more difficult option would be to run a local private Bitcoin network). In order to connect a _bitcoind_ node
to [testnet3](https://en.bitcoin.it/wiki/Testnet) (one of Bitcoin's testnets), you can follow these steps:

1. [Install bitcoind](https://bitcoin.org/en/download):
   this differs depending on your operating system. For the installation instructions on Ubuntu you can
   follow [these steps](https://gist.github.com/rjmacarthy/b56497a81a6497bfabb1).
2. Configure _bitcoind_: This can be done by editing and using the [`bitcoin.conf`](app/src/main/resources/bitcoin.conf)
   file when starting the bicoind daemon. The configuration allows external rpc-based communication with the node, and
   instructs it to communicate with the testnet rather than the mainnet. Furthermore, it orders the node to build an
   index on the blockchain that allows querying even historic transactions. Finally, it instructs the node to send
   notifications to the BAL when it detects a new block or a transaction addressed to one of the Bitcoin wallet's
   addresses. Syncing the whole testnet blockchain (which is done once only) takes about 1-4 hours (depending on the
   hardware, the speed of the network connection, and the availability of peers).
3. Start the pre-configured _bitcoind_ node with the following command:```bitcoind -daemon```
4. Test connection: you can test your connection to a running _bitcoind_ node using the following command
   (make sure to install bitcoin-cli (shipped with _bitcoind_) on the computer where you run this command):

```
bitcoin-cli -getinfo -rpcconnect=<ip address of the node> -rpcport=<port of the node> -rpcuser=<rpc username> -rpcpassword=<rpc password>
```