# How to Run the Blockchain nodes

How to run Arbitrum, Geth, and Prometheus Monitoring with Docker Compose

# With Portainer

TBD

# Docker Compose

## Create Volumes and Networks

```
docker network create blockchain
docker volume create arbitrum
docker volume create geth
docker volume create prometheus-data
docker volume create prometheus-config
```

## Configuration Files

copy Prometheus config to docker volume

```
cp PATH_TO_REPO/blockchain/monitor/promethues.yml to /var/lib/docker/volumes/prometheus-config/_data/
```

configure your Etherium RPC URL in PATH_TO_REPO/blockchain/geth/docker-compose.yml

Replace _YOUR_L1_RPC_URL_ with your rpc URL

## Run the Stacks

```
cd PATH_TO_REPO/blockchain/

# Monitoring stack
cd monitor
docker-compose up -d
cd ..

# Geth Stack
cd geth/
docker-compose up -d
cd ..

# Arbitrum
cd arbitrum/
docker-compose up -d
cd ..
```

## Test Geth

Configure your GETH Url

```
export L1_URL='https://YOUR_GETH_IP:8545/'
```

Send Test Requests to verify

```
curl --location --request POST $L1_URL \
--header 'Content-Type: application/json' \
--data-raw '{
        "jsonrpc":"2.0",
        "method":"web3_clientVersion",
        "params":[],
        "id":1
}'

curl --location --request POST $L1_URL \
--header 'Content-Type: application/json' \
--data-raw '{
        "jsonrpc":"2.0",
        "method":"eth_syncing",
        "params":[],
        "id":1
}'
```
## Test Aribtrum

Configure your ARBITRUM RPC Url

```
export ARB_URL='https://YOUR_ARBITRUM_IP:8547/'
```

Send Test Requests to verify
```
echo ""
echo "net_version"
echo ""
curl $ARB_URL \
  -X POST \
  -H "Content-Type: application/json" \
  --data '{"jsonrpc":"2.0","method":"net_version","params":[],"id":67}'


echo "" 
echo " eth_chainId"
curl $ARB_URL \
  -X POST \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","method":"eth_chainId","params":[],"id":3}'

echo "" 
echo " eth_blockByNumber - latest"
echo "" 
curl $ARB_URL \
  -X POST \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","method":"eth_getBlockByNumber","params":["latest", true],"id":0}'

```

## Test Prometheus

Open the Prometheus Status Page. Click on Targets. Each scrape target should show as "up"

http://YOUR_NODE_IP:9090