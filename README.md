# intension of this read is to keep track of coding structure of lightning network based on the local setup tutorial post below.


through lnd tutorial.
http://dev.lightning.community/tutorial/01-lncli/index.html

it works with testnet from btcd.
<testnet>
mining fund (btcd) -> alice -> bob -> charlie

## Note: required knowledge:
protobuf rpc with go
https://github.com/minaandrawos/Go-Protobuf-Examples.git
this project gives bootstrap in case if you are not familiar with it




### q1 how generation of address works



first run client and d 


```
alice$ lnd --rpcport=10001 --peerport=10011 --restport=8001
```

go to lnd.go func main: 443
lndMain() -> loadConfig() -> config.go
starts program and read ports


```
alice$ lncli --rpcserver=localhost:10001 --no-macaroons create
```

go to commands.go: func create 732

func create -> main.go:41 getWalletUnlockerClient() -> getClientconn() ->(json) \rpc.swagger.json 181












# q2 how alice creares paymentchannel to bob

-"Creating the P2P Network
Now that Alice and Charlie have some simnet Bitcoin, let’s start connecting them together.

Connect Alice to Bob:"

```
# Get Bob's identity pubkey:
bob$ lncli-bob getinfo
{
    ----->"identity_pubkey": <BOB_PUBKEY>,
    "alias": "",
    "num_pending_channels": 0,
    "num_active_channels": 0,
    "num_peers": 0,
    "block_height": 450,
    "block_hash": "2a84b7a2c3be81536ef92cf382e37ab77f7cfbcf229f7d553bb2abff3e86231c",
    "synced_to_chain": true,
    "testnet": false,
    "chains": [
        "bitcoin"
    ]
}

# Connect Alice and Bob together
alice$ lncli-alice connect <BOB_PUBKEY>@localhost:10012
{
    "peer_id": 0
}

```

# a2

first, keet track of the method of fetching Bob's pubKey

lncli-bob getinfo(lncli-bob is alias of lncli)

1. go to lnclid/commands.go 



from lnrpc/rpc.proto
data serialization is done by google protobuf

```
  /** lncli: `getinfo`
    GetInfo returns general information concerning the lightning node including
    it's identity pubkey, alias, the chains it is connected to, and information
    concerning the number of open+pending channels.
    */
    rpc GetInfo (GetInfoRequest) returns (GetInfoResponse) {
        option (google.api.http) = {
            get: "/v1/getinfo"
        };
    }


```


2. 
