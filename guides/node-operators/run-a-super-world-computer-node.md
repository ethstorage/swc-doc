# Run a Super World Computer node

This guide will help you get a Super World Computer node up and running.

## Software dependencies

| Dependency                                                    | Version  | Version Check Command |
| ------------------------------------------------------------- | -------- | --------------------- |
| [git](https://git-scm.com/)                                   | `^2`     | `git --version`       |
| [go](https://go.dev/)                                         | `^1.21`  | `go version`          |
| [make](https://linux.die.net/man/1/make)                      | `^3`     | `make --version`      |

## Beta Testnet

### Steps

1. Follow the following steps to build a node. The steps are basically the same as in [Optimism's documentation](https://docs.optimism.io/builders/node-operators/tutorials/node-from-source), the only difference is that here we use the `beta_testnet` branch of both [our optimism fork](https://github.com/ethstorage/optimism/tree/beta_testnet) and [our op-geth fork](https://github.com/ethstorage/op-geth/tree/beta_testnet) instead.

- 1.1 Build `op-geth`
    ```bash
    git clone -b beta_testnet https://github.com/ethstorage/op-geth.git
    pushd op-geth && make geth && popd
    ```
- 1.2 Build `op-node`
    ```bash
    git clone -b beta_testnet https://github.com/ethstorage/optimism.git
    pushd optimism && make op-node && popd
    ```

2. Setup `op-geth`:

    ```bash
        # assume optimism and op-geth repo are located at ./optimism and ./op-geth

        cd op-geth
        # prepare beta_testnet_genesis.json
        curl -LO https://raw.githubusercontent.com/ethstorage/pm/main/L2/assets/beta_testnet_genesis.json
        ./build/bin/geth init --datadir=datadir --state.scheme hash beta_testnet_genesis.json

        openssl rand -hex 32 > jwt.txt

        # We don't specify `--rollup.sequencerhttp` since it's for testing blob archiver only.
        # The rpc port is the default one: 8545.
        ./build/bin/geth   --datadir ./datadir   --http   --http.corsdomain="*"   --http.vhosts="*"   --http.addr=0.0.0.0   --http.api=web3,debug,eth,txpool,net,engine   --ws   --ws.addr=0.0.0.0   --ws.port=8546   --ws.origins="*"   --ws.api=debug,eth,txpool,net,engine   --syncmode=full   --gcmode=archive   --nodiscover   --maxpeers=0   --networkid=3335   --authrpc.vhosts="*"   --authrpc.addr=0.0.0.0   --authrpc.port=8551   --authrpc.jwtsecret=./jwt.txt   --rollup.disabletxpoolgossip=true
    ```

3. Setup `op-node`:
    ```bash
        # assume optimism and op-geth repo are located at ./optimism and ./op-geth
        # copy jwt.txt from the op-geth directory above to optimism/op-node
        cp op-geth/jwt.txt optimism/op-node 
        cd optimism/op-node

        export L1_RPC_KIND=basic
        export L1_RPC_URL=http://88.99.30.186:8545
        export L1_BEACON_URL=http://88.99.30.186:3500
        # prepare beta_testnet_rollup.json
        curl -LO https://raw.githubusercontent.com/ethstorage/pm/main/L2/assets/beta_testnet_rollup.json

        mkdir safedb
        # Ensure to replace --p2p.static with the sequencer's address.
        # Note: p2p is enabled for unsafe block.
        ./bin/op-node   --l2=http://localhost:8551   --l2.jwt-secret=./jwt.txt   --verifier.l1-confs=4   --rollup.config=./beta_testnet_rollup.json   --rpc.addr=0.0.0.0   --rpc.port=8547   --p2p.static=/ip4/5.9.87.214/tcp/9003/p2p/16Uiu2HAm2w9ZsnP58zzGpPXGuCH8j6w9ecwA3uwXhkXxJniJEbUX --p2p.listen.ip=0.0.0.0 --p2p.listen.tcp=9003 --p2p.listen.udp=9003  --p2p.no-discovery --p2p.sync.onlyreqtostatic --rpc.enable-admin   --l1=$L1_RPC_URL   --l1.rpckind=$L1_RPC_KIND --l1.beacon=$L1_BEACON_URL --l1.beacon-archiver=http://65.108.236.27:9645 --safedb.path=safedb
    ```

---

## Devnet

### Steps

1. Follow the following steps to build a node. The steps are basically the same as in [Optimism's documentation](https://docs.optimism.io/builders/node-operators/tutorials/node-from-source), the only difference is that here we use the `devnet` branch of both [our optimism fork](https://github.com/ethstorage/optimism/tree/devnet) and [our op-geth fork](https://github.com/ethstorage/op-geth/tree/devnet) instead.

- 1.1 Build `op-geth`
    ```bash
    git clone -b devnet https://github.com/ethstorage/op-geth.git
    pushd op-geth && make geth && popd
    ```
- 1.2 Build `op-node`
    ```bash
    git clone -b devnet https://github.com/ethstorage/optimism.git
    pushd optimism && make op-node && popd
    ```

2. Setup `op-geth`:

    ```bash
        # assume optimism and op-geth repo are located at ./optimism and ./op-geth

        cd op-geth
        # prepare devnet_genesis.json
        curl -LO https://raw.githubusercontent.com/ethstorage/pm/main/L2/assets/devnet_genesis.json

        build/bin/geth init --datadir=datadir devnet_genesis.json

        openssl rand -hex 32 > jwt.txt

        # We don't specify `--rollup.sequencerhttp` since it's for testing blob archiver only.
        # The rpc port is the default one: 8545.
        ./build/bin/geth   --datadir ./datadir   --http   --http.corsdomain="*"   --http.vhosts="*"   --http.addr=0.0.0.0   --http.api=web3,debug,eth,txpool,net,engine   --ws   --ws.addr=0.0.0.0   --ws.port=8546   --ws.origins="*"   --ws.api=debug,eth,txpool,net,engine   --syncmode=full   --gcmode=archive   --nodiscover   --maxpeers=0   --networkid=42069   --authrpc.vhosts="*"   --authrpc.addr=0.0.0.0   --authrpc.port=8551   --authrpc.jwtsecret=./jwt.txt   --rollup.disabletxpoolgossip=true --enablel2blob
    ```
 
 3. Setup `op-node`:
    ```bash
        # assume optimism and op-geth repo are located at ./optimism and ./op-geth
        # copy jwt.txt from the op-geth directory above to optimism/op-node
        cp op-geth/jwt.txt optimism/op-node
        cd optimism/op-node

        # prepare devnet_rollup.json
        curl -LO https://raw.githubusercontent.com/ethstorage/pm/main/L2/assets/devnet_rollup.json
        export L1_RPC_KIND=basic
        export L1_RPC_URL=http://88.99.30.186:8545
        export L1_BEACON_URL=http://88.99.30.186:3500

        # Ensure to replace --p2p.static with the sequencer's address.
        # Note: p2p is enabled for unsafe block.
         ./bin/op-node   --l2=http://localhost:8551   --l2.jwt-secret=./jwt.txt   --verifier.l1-confs=4   --rollup.config=./devnet_rollup.json   --rpc.addr=0.0.0.0   --rpc.port=8547   --p2p.static=/ip4/65.109.20.29/tcp/9003/p2p/16Uiu2HAmP3KorAMS1DC5SdDEcNGwhMFKuoyvZzBSWXdqysZgrxQ7 --p2p.listen.ip=0.0.0.0 --p2p.listen.tcp=9003 --p2p.listen.udp=9003  --p2p.no-discovery --p2p.sync.onlyreqtostatic --rpc.enable-admin   --l1=$L1_RPC_URL   --l1.rpckind=$L1_RPC_KIND --l1.beacon=$L1_BEACON_URL --l1.beacon-archiver=http://65.108.236.27:9645
    ```
 

    