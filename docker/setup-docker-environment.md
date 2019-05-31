### Download default configuration file at [https://raw.githubusercontent.com/elastos/Elastos.ELA/release_v0.3.2/docs/dpos_config.json.sample](https://raw.githubusercontent.com/elastos/Elastos.ELA/release_v0.3.2/docs/dpos_config.json.sample) and copy to `docker/ela/config.json`
```bash
cd docker/ela
wget https://raw.githubusercontent.com/elastos/Elastos.ELA/release_v0.3.2/docs/dpos_config.json.sample
mv dpos_config.json.sample config.json
```

### Download ela-cli binary at [https://github.com/elastos/Elastos.ELA/releases/download/v0.3.2/ela-cli](https://github.com/elastos/Elastos.ELA/releases/download/v0.3.2/ela-cli) and copy to `docker/ela/ela-cli`
```bash
cd docker/ela
wget https://github.com/elastos/Elastos.ELA/releases/download/v0.3.2/ela-cli
```

### Create a wallet using ela-cli
- Creating a wallet creates a keystore.dat file. This file is used to store your node's public key and the DPoS supernode uses this file to complete node conmmunication
- The password encrypts keystore.dat so it needs to be added when running your ela node later on. It is recommended to set up a more complex password in production environment
```bash
# This creates a keystore.dat file with password "elastos"
./ela-cli wallet create -p elastos 
```

### Take a note of your public key associated to your wallet
- This is the node public key that you'll associate with when you register for a supernode via Elastos Wallet. If you have already registered for a supernode, make sure to update the node public key with the result you get here as that's how you will be able to link your wallet to your supernode
- By default when you register for a supernode via Elastos Wallet, it automatically puts in your wallet public key. So, make sure to change this to your node public key instead by updating info on your supernode from the wallet
```bash
./ela-cli wallet account -p elastos
```

### Make changes to `ela/config.json`
- Change "IPAddress" to your server public IP or domain name
- "RpcConfiguration" is used to put access restriction to the RPC interface
- "WhiteIPList" is an IP whitelist that allows access to this ela node. "0.0.0.0" means no access restriction and anyone can access this node from the network
- "User" and "Pass" are the username and password for accessing the RPC interface. If set to "", you can access it without entering username and password
- "WhiteIPList", "User" and "Pass" must all be met to access the RPC interface
```json
{
    "Configuration": {
      "DPoSConfiguration": {
        "EnableArbiter": true,
        "IPAddress": "127.0.0.1"
      },
      "RpcConfiguration": {
        "User": "user",
        "Pass": "password",
        "WhiteIPList": ["0.0.0.0"]
      }
    }
}
```

### Make changes to `did/config.json`
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
    "0.0.0.0"
  ]
}
```

### Make changes to `token/config.json`
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
    "0.0.0.0"
  ]
}
```

### Make changes to `carrier/bootstrapd.conf`
- Update bootstrap nodes list under the section "bootstrap_nodes" if you need to(so you can connect to elastos carrier nodes)
- Set external IP to turn server explicitly: Some Linux VPS servers, for example, servers from AWS, can't fetch public IP address directly by itself, so you have manually update the public IP address of item external_ip under the section "turn"