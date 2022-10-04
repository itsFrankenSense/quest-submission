## Quests

**1. Figure out how to mint an NFT to yourself by sending a transaction using the Flow CLI, like we did today when we set up our collection. You will also likely have to pass an argument as well.
Helpful tip: Remember that only the owner of the contract has access to the Minter resource. This works in our favor because the signer of the transaction will be the one who deployed the contract, so we have access to the Minter.**
<br> - https://testnet.flowscan.org/transaction/5a4a985543e5e3c824d5b98cedfe839de9b537226247918297d214932f120bea

**2. Run a script to read the new totalSupply using the Flow CLI**
<br>
``` Cadence
import CryptoPoops from "../contracts/CryptoPoops.cdc"

pub fun main(): UInt64 {
  return CryptoPoops.totalSupply
}
```

**3. Run a script to read the ids of NFTs in someone's collection using the Flow CLI**
<br> 
```Cadence

import CryptoPoops from "../contracts/CryptoPoops.cdc"

pub fun main(address: Address): [UInt64] {
  let publicReference = getAccount(address).getCapability(/public/Collection)
                          .borrow<&CryptoPoops.Collection{CryptoPoops.CollectionPublic}>()
                          ?? panic("This account does not have a Collection")

  return publicReference.getIDs()

}
```
<br>
**Command:** flow scripts execute ./scripts/read_ids.cdc 0x2d7a5f7643681c7a --network=testnet
<br>
**Result:** [1, 112128356, 0, 112128087]

**4. Run a script to read a specific NFT's metadata from someone's collection using the Flow CLI**
<br>
``` Cadence
import CryptoPoops from "../contracts/CryptoPoops.cdc"

pub fun main(address: Address, id: UInt64): &CryptoPoops.NFT {
  let publicReference = getAccount(address).getCapability(/public/Collection)
                          .borrow<&CryptoPoops.Collection{CryptoPoops.CollectionPublic}>()
                          ?? panic("This account does not have a Collection")

  let pubref: &CryptoPoops.NFT = publicReference.borrowAuthNFT(id: id)
  
  log(pubref.id)
  log(pubref.name)
  log(pubref.favouriteFood)
  log(pubref.luckyNumber)

  return pubref

}
```
<br> 
Command: flow scripts execute ./scripts/Read_Metadata.cdc 0x2d7a5f7643681c7a 1 --network=testnet
<br>
Result: A.2d7a5f7643681c7a.CryptoPoops.NFT(uuid: 112129322, id: 1, name: "FrankenSense", favouriteFood: "Pizza With Pineapple", luckyNumber: 15)


**5. Run a script to read the GoatedGoats totalSupply on Flow Mainnet. Their contract lives here: https://flow-view-source.com/mainnet/account/0x2068315349bdfce5/contract/GoatedGoats**
<br>
``` Cadence
import GoatedGoats from 0x2068315349bdfce5

pub fun main(): UInt64 {
  return GoatedGoats.totalSupply

}
```
<br>
Command:flow scripts execute ./scripts/goats_total_supply.cdc --network=mainnet                              
<br>
Result:18476
<br>

**6. Figure out how to read someone's GoatedGoats NFTs from their collection and run a script using the Flow CLI to do it.**
<br> 
``` Cadence
import GoatedGoats from 0x2068315349bdfce5

pub fun main(address: Address): [UInt64] {
  let publicReference = getAccount(address).getCapability(/public/GoatCollection)
                          .borrow<&GoatedGoats.Collection{GoatedGoats.GoatCollectionPublic}>()
                          ?? panic("This account does not have a Collection")

  return publicReference.getIDs()

}
```

<br> 
Command: cadence % flow scripts execute ./scripts/goats_ids.cdc 0xd3a53faac402a0fb --network=mainnet
<br>
Result: [18475, 18476, 18473, 18464, 18462]
