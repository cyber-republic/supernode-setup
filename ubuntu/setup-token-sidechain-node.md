### Create a directory to work off of: `~/node/token`:
  ```bash
  mkdir -p ~/node/token;
  cd ~/node/token
  ```

### Download token binary at [https://github.com/elastos/Elastos.ELA.SideChain.Token/releases/download/v0.1.2/token](https://github.com/elastos/Elastos.ELA.SideChain.Token/releases/download/v0.1.2/token) and copy to `~/node/token/token`
  ```bash
  wget https://github.com/elastos/Elastos.ELA.SideChain.Token/releases/download/v0.1.2/token;
  chmod +x token
  ```

### Download default configuration file at [https://raw.githubusercontent.com/elastos/Elastos.ELA.SideChain.Token/master/docs/mainnet_config.json.sample](https://raw.githubusercontent.com/elastos/Elastos.ELA.SideChain.Token/master/docs/mainnet_config.json.sample) and copy to `~/node/token/config.json`
  ```bash
  wget https://raw.githubusercontent.com/elastos/Elastos.ELA.SideChain.Token/master/docs/mainnet_config.json.sample;
  mv mainnet_config.json.sample config.json
  ```

### Modify token configuration file: `config.json`
- Change "IPAddress" to your server public IP or domain name
- "RpcConfiguration" is used to put access restriction to the RPC interface
- "WhiteIPList" is an IP whitelist that allows access to this ela node. "0.0.0.0" means no access restriction and anyone can access this node from the network
- "User" and "Pass" are the username and password for accessing the RPC interface. If set to "", you can access it without entering username and password
- "WhiteIPList", "User" and "Pass" must all be met to access the RPC interface
  ```json
  {
    "SPVDisableDNS": false,
    "SPVPermanentPeers": [
      "localhost:20338"
    ],
    "EnableRPC": true,
    "RPCUser": "user",
    "RPCPass": "password",
    "RPCWhiteList": [
      "127.0.0.1"
    ]
  }
  ```

### Run token node 
1. The token node startup command
  ```bash
  # This will run the ./token program in the background and it sends all the 
  # outputted logs to /dev/null and only captures error logs to a file
  # called "output"
  nohup ./token > /dev/null 2>output & 
  ```

2. After the node is started, you can check the node information such as node height, version, etc with RPC interface
  ```bash
  # This assumes that RPC username and password are not set and default
  # port is used for RPC
  curl -X POST http://localhost:20616 -H 'Content-Type: application/json' -d '{"method": "getnodestate"}' 
  # This assumes that RPC username and password are set with the following
  # and the HttpJsonPort is configured to be 20616
  curl --user User:Password -X POST http://localhost:20616 -H 'Content-Type: application/json' -d '{"method": "getnodestate"}'
  ```

3. You can check out other RPC commands by looking at the documentation at [https://github.com/elastos/Elastos.ELA.SideChain.Token/blob/master/docs/jsonrpc_apis.md](https://github.com/elastos/Elastos.ELA.SideChain.Token/blob/master/docs/jsonrpc_apis.md)

4. The logs for the token node program is located at elastos_token/logs directory

  ```
  cat elastos_token/logs/2019-05-29_19.29.19.log | head -10
  ```

  Should output something like:
  ```
  2019-05-29 19:29:19.996 [INF] ELAD /home/kpachhai/dev/src/github.com/elastos/Elastos.ELA.SideChain.Token/main.go:58: Node version: v0.1.1-7-g3fd3
  2019-05-29 19:29:19.996 [INF] ELAD /home/kpachhai/dev/src/github.com/elastos/Elastos.ELA.SideChain.Token/main.go:59: go version go1.11.5 linux/amd64
  2019-05-29 19:29:19.996 [INF] ELAD /home/kpachhai/dev/src/github.com/elastos/Elastos.ELA.SideChain.Token/main.go:64: 1. BlockChain init
  2019-05-29 19:29:20.019 [INF] ELAD /home/kpachhai/dev/src/github.com/elastos/Elastos.ELA.SideChain.Token/main.go:73: 2. SPV module init
  2019-05-29 19:29:20.067 [INF] SPVS /home/kpachhai/dev/src/github.com/elastos/Elastos.ELA.SideChain.Token/vendor/github.com/elastos/Elastos.ELA.SPV/sdk/service.go:464: SPV service started...
  2019-05-29 19:29:20.067 [INF] ELAD /home/kpachhai/dev/src/github.com/elastos/Elastos.ELA.SideChain.Token/main.go:137: 3. Start the P2P networks
  2019-05-29 19:29:20.069 [INF] ELAD /home/kpachhai/dev/src/github.com/elastos/Elastos.ELA.SideChain.Token/main.go:160: 4. --Initialize pow service
  2019-05-29 19:29:20.069 [INF] ELAD /home/kpachhai/dev/src/github.com/elastos/Elastos.ELA.SideChain.Token/main.go:181: 5. --Start the RPC service
  2019-05-29 19:29:20.072 [INF] SPVS /home/kpachhai/dev/src/github.com/elastos/Elastos.ELA.SideChain.Token/vendor/github.com/elastos/Elastos.ELA.SPV/sync/manager.go:244: New valid peer 127.0.0.1:10015 (outbound)
  2019-05-29 19:29:20.072 [INF] SPVS /home/kpachhai/dev/src/github.com/elastos/Elastos.ELA.SideChain.Token/vendor/github.com/elastos/Elastos.ELA.SPV/sync/manager.go:197: Syncing to block height 1226 from peer 127.0.0.1:10015
  ```