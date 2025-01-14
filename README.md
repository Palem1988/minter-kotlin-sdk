## Minter Blockchain Kotlin SDK (unofficial)

### About
Minter Blockchain Kotlin SDK [Minter.network](https://minter.network) (unofficial).
Kotlin SDK was originally developed for internal parser of service [Interсhain.Zone](https://minter.interchain.zone) 

![Minter Blockchain Kotlin SDK](static/minter-kotlin-sdk-header.png "Minter Blockchain Kotlin SDK")

#### Simple using
```kotlin
import MinterApi.MinterApi

class Main {
    companion object {
        @JvmStatic
        fun main(args: Array<String>) {
println("Hello from Kotlin")
            val minterApi = MinterApi("https://minter-node-2.testnet.minter.network:8841")
            val status = minterApi.getStatus()
            println(status) // Status(height=977154, datetime=2019-07-11T21:26:22.119+03:00, network=minter-mainnet-1)
        }
    }
}
```

#### Node (getCandidate)
Returns candidate’s info by provided public_key.
```kotlin
val node = minterApi.getNodeRaw("Mp01cc99ae5a349ecaeef187dcbb12816bf2b3d8eae80f654034b21213aa445b2c")
println(node) // NodeRaw(reward=Mx0a76...dd711, owner=Mx5ebe...7799, pub_key=Mp01cc...5b2c, commission=10, crblock=4)
```


#### Coin (getCoinInfo)
Returns information about coin.
```kotlin
val coin = minterApi.getCoin("ROBOT")
println(coin) //  Coin(id=null, symbol=ROBOT, length=5, name=Coin for robots, creater=null, crr=80, ...)
```

#### Wallet (getBalance)
Returns coins list, balance and transaction count (for nonce) of an address.
```kotlin
val wallet = minterApi.getAddress("Mxcd633fd8ec1b0a181627dfd72f9ba25e93f0c899")
println(wallet) // Wallet(id=null, address=Mxcd633fd8ec1b0a181627dfd72f9ba25e93f0c899, count_txs=17, balance={BTCSECURE=5.3534213964374E-5, BIP=4245.51470327139, SATOSHI=888.8888})       
```

#### Block (getBlock)
Returns block data at given height.
```kotlin
val block = minterApi.getBlockRaw(2)
println(block) /* BlockRaw(height=2, time=2019-05-15T18:05:02.395+03:00, num_txs=3, total_txs=3, reward=333.0, size=4310, proposer=Mp...,
              transaction=[TransactionRaw(hash=Mt..., height=2, type=5, from=Mx..., to=null, node=, stake=15000000000000000000000000,
                coin=BTCSECURE, amount=1.5E7, gas_price=1, commission=null, payload=false, gas=100000, gascoin=BIP)},
                "payload":"","gas":"10000","from":"Mx...","service_data":"","gas_coin":"BIP","type":6,"raw_tx":"******","nonce":"1",
                "hash":"Mt.....","tags":{"tx.type":"06","tx.from":"****"}}])
                */          
```

#### estimateCoinSell (estimateCoinSell)
Return estimate of sell coin transaction
```kotlin
val estimateCoinSell = minterApi.estimateCoinSell("ROBOT", 1.0, "BIP")
println(estimateCoinSell) // EstimateCoinSell(willGet=21.908222878076707, commission=0.1)
          
```

#### estimateCoinBuy (estimateCoinBuy)
Return estimate of buy coin transaction.
```kotlin
val estimateCoinBuy = minterApi.estimateCoinBuy("BIP", 1.0, "BTCSECURE")
println(estimateCoinBuy) // EstimateCoinBuy(willPay=0.04564504450213553, commission=0.1)    
```

#### Advanced Use Example
```kotlin
val newNode = minterApi.getNode("Mp01cc99ae5a349ecaeef187dcbb12816bf2b3d8eae80f654034b21213aa445b2c", 0, {
    val reward = minterApi.getAddress(it)
//  println(reward)
    0
    }, {
        val owner = minterApi.getAddress(it)
        if (owner != null) {
            owner.balance.forEach { coin, amount ->
                if (coin != "BIP") {
                    val priceInBip = minterApi.estimateCoinSell(coin, amount, "BIP")!!.willGet
                    println ("$amount $coin = $priceInBip BIP") // 100.00006004578127 BTCSECURE = 4.564748939706455 BIP
                } else {
                    println ("$amount $coin")
                }
            }
        }
    0
    }
)
```



