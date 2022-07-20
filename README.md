# Overview

This contains some useful tools for Bitcoin transaction analysis.

- [`bitcoin-core`](https://bitcoin.org/en/bitcoin-core/) to fetch Bitcoin's blocks and transactions
- [`blocksci`](https://citp.github.io/BlockSci/) to process Bitcoin's blocks and transactions retrieved by `bitcoin-core`
- [`jupyterlab`](https://jupyter.org) for Bitcoin transaction analysis with `blocksci`

# Prerequisites

* `docker`
* `docker-compose`
* `tmux` would be useful for `bitcoin-core` and `blocksci`
* Large disk space (> 1 TB)

# Usage

Be familiarized with `docker`, `docker-compose`, `tmux`, `bitcoind`, `blocksci`, and `jupyterlab`. The following instruction may not be complete.

## bitcoin-core

1. `cd ./bitcoin-core`
1. `docker-compose up --build -d` to fetch Bitcoin's blocks and transactions in `.bitcoin` (It may take several days to sync up)
1. `docker logs bitcoin-core -ft` to see the progress of `bitcoind`
1. `docker-compose down` to halt the container

## blocksci

1. Assume you're in the top directory of this project
1. `cd ./blocksci`
1. `mv ../bitcoin-core/.bitcoin .`
1. `docker-compose up --build -d` to cluster Bitcoin addresses found in `.bitcoin` (This process may take several weeks)
1. `docker logs blocksci -ft` to see the progress of `blockcsi`
1. Once the clustering process completes, the container automatically halts (you can check this with `docker ps` too)
1. `docker cp blocksci:/home/jovyan/config.json ./jupyterlab/docker`

## jupyterlab

1. Assume you're in the top directory of this project
1. `cd ./jupyterlab`
1. `mv ../blocksci/blocksci-output .`
1. `mv ../blocksci/.bitcoin .`
1. `docker-compose up --build -d` to boot a jupyterlab container
1. Access it from your web browser (default port: `8080`). If you're using a server, make sure to open the port specified in `docker-compose.yml`.
1. Try `notebooks/BlockSci Demo.ipynb`
