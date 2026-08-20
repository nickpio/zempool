# Docker installation

This directory holds the Dockerfiles and a `docker-compose.yml`. The images only run Zempool's frontend and backend. You still run a Zcash node yourself.

Zempool talks to **Zebra** (`zebrad`) or **Zakura** (`zakurad`) over JSON-RPC. Do not use zcashd. Do not use Zakura's zcashd-compat sidecar.

The node must not be pruned. Address history only works if the node exposes insight-style address RPCs. `MEMPOOL_BACKEND` should stay `"none"`. There is no Electrum or Esplora for Zcash in v1.

Jump to a section in this doc:
- [Point at Zebra or Zakura](#point-at-zebra-or-zakura)
- [Further Configuration](#further-configuration)

## Point at Zebra or Zakura

Enable RPC on the node. Mainnet RPC is port 8232. Testnet is 18232. Cookie auth is the usual setup:

- Zakura: `~/.cache/zakura/.cookie`
- Zebra: `~/.cache/zebra/.cookie`

`docker-compose.yml` defaults to the Docker host gateway so the `api` container can reach a node on the host:

```yaml
  api:
    environment:
      MEMPOOL_BACKEND: "none"
      CORE_RPC_HOST: "172.27.0.1"
      CORE_RPC_PORT: "8232"
      CORE_RPC_COOKIE: "true"
      CORE_RPC_COOKIE_PATH: "/path/on/the/container/to/.cookie"
```

If you use username and password instead of a cookie, set `CORE_RPC_USERNAME` and `CORE_RPC_PASSWORD` and leave cookie off. Change the host IP if your node is not on the Docker gateway.

The node must be synced. Then:

```bash
docker-compose up
```

Zempool should be at http://localhost. Graphs fill in as new transactions show up.

## Further Configuration

Optionally, you can override any other backend settings from `mempool-config.json`.

Below we list all settings from `mempool-config.json` and the corresponding overrides you can make in the `api` > `environment` section of `docker-compose.yml`. 

<br/>

`mempool-config.json`:
```json
  "MEMPOOL": {
    "NETWORK": "mainnet",
    "BACKEND": "electrum",
    "ENABLED": true,
    "HTTP_PORT": 8999,
    "SPAWN_CLUSTER_PROCS": 0,
    "API_URL_PREFIX": "/api/v1/",
    "POLL_RATE_MS": 2000,
    "CACHE_DIR": "./cache",
    "CLEAR_PROTECTION_MINUTES": 20,
    "RECOMMENDED_FEE_PERCENTILE": 50,
    "BLOCK_WEIGHT_UNITS": 4000000,
    "INITIAL_BLOCKS_AMOUNT": 8,
    "MEMPOOL_BLOCKS_AMOUNT": 8,
    "BLOCKS_SUMMARIES_INDEXING": false,
    "USE_SECOND_NODE_FOR_MINFEE": false,
    "EXTERNAL_ASSETS": [],
    "STDOUT_LOG_MIN_PRIORITY": "info",
    "INDEXING_BLOCKS_AMOUNT": false,
    "AUTOMATIC_POOLS_UPDATE": false,
    "POOLS_JSON_URL": "https://raw.githubusercontent.com/mempool/mining-pools/master/pools-v2.json",
    "POOLS_JSON_TREE_URL": "https://api.github.com/repos/mempool/mining-pools/git/trees/master",
    "POOLS_UPDATE_DELAY": 604800,
    "CPFP_INDEXING": false,
    "MAX_BLOCKS_BULK_QUERY": 0,
    "DISK_CACHE_BLOCK_INTERVAL": 6,
    "PRICE_UPDATES_PER_HOUR": 1
  },
```

Corresponding `docker-compose.yml` overrides:
```yaml
  api:
    environment:
      MEMPOOL_NETWORK: ""
      MEMPOOL_BACKEND: ""
      BACKEND_HTTP_PORT: ""
      MEMPOOL_SPAWN_CLUSTER_PROCS: ""
      MEMPOOL_API_URL_PREFIX: ""
      MEMPOOL_POLL_RATE_MS: ""
      MEMPOOL_CACHE_DIR: ""
      MEMPOOL_CLEAR_PROTECTION_MINUTES: ""
      MEMPOOL_RECOMMENDED_FEE_PERCENTILE: ""
      MEMPOOL_BLOCK_WEIGHT_UNITS: ""
      MEMPOOL_INITIAL_BLOCKS_AMOUNT: ""
      MEMPOOL_MEMPOOL_BLOCKS_AMOUNT: ""
      MEMPOOL_BLOCKS_SUMMARIES_INDEXING: ""
      MEMPOOL_USE_SECOND_NODE_FOR_MINFEE: ""
      MEMPOOL_EXTERNAL_ASSETS: ""
      MEMPOOL_STDOUT_LOG_MIN_PRIORITY: ""
      MEMPOOL_INDEXING_BLOCKS_AMOUNT: ""
      MEMPOOL_AUTOMATIC_POOLS_UPDATE: ""
      MEMPOOL_POOLS_JSON_URL: ""
      MEMPOOL_POOLS_JSON_TREE_URL: ""
      MEMPOOL_POOLS_UPDATE_DELAY: ""
      MEMPOOL_CPFP_INDEXING: ""
      MEMPOOL_MAX_BLOCKS_BULK_QUERY: ""
      MEMPOOL_DISK_CACHE_BLOCK_INTERVAL: ""
      MEMPOOL_PRICE_UPDATES_PER_HOUR: ""
      ...
```

`CPFP_INDEXING` enables indexing CPFP (Child Pays For Parent) information for the last `INDEXING_BLOCKS_AMOUNT` blocks.

<br/>

`mempool-config.json`:
```json
  "CORE_RPC": {
    "HOST": "127.0.0.1",
    "PORT": 8232,
    "USERNAME": "mempool",
    "PASSWORD": "mempool",
    "TIMEOUT": 60000,
    "COOKIE": false,
    "COOKIE_PATH": ""
  },
```

Corresponding `docker-compose.yml` overrides:
```yaml
  api:
    environment:
      CORE_RPC_HOST: ""
      CORE_RPC_PORT: ""
      CORE_RPC_USERNAME: ""
      CORE_RPC_PASSWORD: ""
      CORE_RPC_TIMEOUT: 60000
      CORE_RPC_COOKIE: false
      CORE_RPC_COOKIE_PATH: ""
      ...
```

<br/>

`mempool-config.json`:
```json
  "ELECTRUM": {
    "HOST": "127.0.0.1",
    "PORT": 50002,
    "TLS_ENABLED": true
  },
```

Corresponding `docker-compose.yml` overrides:
```yaml
  api:
    environment:
      ELECTRUM_HOST: ""
      ELECTRUM_PORT: ""
      ELECTRUM_TLS_ENABLED: ""
      ...
```

<br/>

`mempool-config.json`:
```json
  "ESPLORA": {
    "REST_API_URL": "http://127.0.0.1:3000",
    "UNIX_SOCKET_PATH": "/tmp/esplora-socket",
    "RETRY_UNIX_SOCKET_AFTER": 30000
  },
```

Corresponding `docker-compose.yml` overrides:
```yaml
  api:
    environment:
      ESPLORA_REST_API_URL: ""
      ESPLORA_UNIX_SOCKET_PATH: ""
      ESPLORA_RETRY_UNIX_SOCKET_AFTER: ""
      ...
```

<br/>

`mempool-config.json`:
```json
  "SECOND_CORE_RPC": {
    "HOST": "127.0.0.1",
    "PORT": 8232,
    "USERNAME": "mempool",
    "PASSWORD": "mempool",
    "TIMEOUT": 60000,
    "COOKIE": false,
    "COOKIE_PATH": ""
  },
```

Corresponding `docker-compose.yml` overrides:
```yaml
  api:
    environment:
      SECOND_CORE_RPC_HOST: ""
      SECOND_CORE_RPC_PORT: ""
      SECOND_CORE_RPC_USERNAME: ""
      SECOND_CORE_RPC_PASSWORD: ""
      SECOND_CORE_RPC_TIMEOUT: ""
      SECOND_CORE_RPC_COOKIE: false
      SECOND_CORE_RPC_COOKIE_PATH: ""
      ...
```

<br/>

`mempool-config.json`:
```json
  "DATABASE": {
    "ENABLED": true,
    "HOST": "127.0.0.1",
    "PORT": 3306,
    "DATABASE": "mempool",
    "USERNAME": "mempool",
    "PASSWORD": "mempool"
  },
```

Corresponding `docker-compose.yml` overrides:
```yaml
  api:
    environment:
      DATABASE_ENABLED: ""
      DATABASE_HOST: ""
      DATABASE_PORT: ""
      DATABASE_DATABASE: ""
      DATABASE_USERNAME: ""
      DATABASE_PASSWORD: ""
      DATABASE_TIMEOUT: ""
      ...
```

<br/>

`mempool-config.json`:
```json
  "SYSLOG": {
    "ENABLED": true,
    "HOST": "127.0.0.1",
    "PORT": 514,
    "MIN_PRIORITY": "info",
    "FACILITY": "local7"
  },
```

Corresponding `docker-compose.yml` overrides:
```yaml
  api:
    environment:
      SYSLOG_ENABLED: ""
      SYSLOG_HOST: ""
      SYSLOG_PORT: ""
      SYSLOG_MIN_PRIORITY: ""
      SYSLOG_FACILITY: ""
      ...
```

<br/>

`mempool-config.json`:
```json
  "STATISTICS": {
    "ENABLED": true,
    "TX_PER_SECOND_SAMPLE_PERIOD": 150
  },
```

Corresponding `docker-compose.yml` overrides:
```yaml
  api:
    environment:
      STATISTICS_ENABLED: ""
      STATISTICS_TX_PER_SECOND_SAMPLE_PERIOD: ""
      ...
```

<br/>

`mempool-config.json`:
```json
  "SOCKS5PROXY": {
    "ENABLED": false,
    "HOST": "127.0.0.1",
    "PORT": "9050",
    "USERNAME": "",
    "PASSWORD": ""
  }
```

Corresponding `docker-compose.yml` overrides:
```yaml
  api:
    environment:
      SOCKS5PROXY_ENABLED: ""
      SOCKS5PROXY_HOST: ""
      SOCKS5PROXY_PORT: ""
      SOCKS5PROXY_USERNAME: ""
      SOCKS5PROXY_PASSWORD: ""
      ...
```

<br/>

`mempool-config.json`:
```json
  "LIGHTNING": {
    "ENABLED": false
    "BACKEND": "lnd"
    "TOPOLOGY_FOLDER": ""
    "STATS_REFRESH_INTERVAL": 600
    "GRAPH_REFRESH_INTERVAL": 600
    "LOGGER_UPDATE_INTERVAL": 30
  }
```

Corresponding `docker-compose.yml` overrides:
```yaml
  api:
    environment:
      LIGHTNING_ENABLED: false
      LIGHTNING_BACKEND: "lnd"
      LIGHTNING_TOPOLOGY_FOLDER: ""
      LIGHTNING_STATS_REFRESH_INTERVAL: 600
      LIGHTNING_GRAPH_REFRESH_INTERVAL: 600
      LIGHTNING_LOGGER_UPDATE_INTERVAL: 30
      ...
```

<br/>

`mempool-config.json`:
```json
  "LND": {
    "TLS_CERT_PATH": ""
    "MACAROON_PATH": ""
    "REST_API_URL": "https://localhost:8080"
    "TIMEOUT": 10000
  }
```

Corresponding `docker-compose.yml` overrides:
```yaml
  api:
    environment:
      LND_TLS_CERT_PATH: ""
      LND_MACAROON_PATH: ""
      LND_REST_API_URL: "https://localhost:8080"
      LND_TIMEOUT: 10000
      ...
```

<br/>

`mempool-config.json`:
```json
  "CLIGHTNING": {
    "SOCKET": ""
  }
```

Corresponding `docker-compose.yml` overrides:
```yaml
  api:
    environment:
      CLIGHTNING_SOCKET: ""
      ...
```

<br/>

`mempool-config.json`:
```json
  "MAXMIND": {
    "ENABLED": true,
    "GEOLITE2_CITY": "/usr/local/share/GeoIP/GeoLite2-City.mmdb",
    "GEOLITE2_ASN": "/usr/local/share/GeoIP/GeoLite2-ASN.mmdb",
    "GEOIP2_ISP": "/usr/local/share/GeoIP/GeoIP2-ISP.mmdb"
  }
```

Corresponding `docker-compose.yml` overrides:
```yaml
  api:
    environment:
      MAXMIND_ENABLED: true,
      MAXMIND_GEOLITE2_CITY: "/backend/GeoIP/GeoLite2-City.mmdb",
      MAXMIND_GEOLITE2_ASN": "/backend/GeoIP/GeoLite2-ASN.mmdb",
      MAXMIND_GEOIP2_ISP": "/backend/GeoIP/GeoIP2-ISP.mmdb"
      ...
```
