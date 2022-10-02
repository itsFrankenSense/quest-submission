## Quests

**1. What does "force casting" with as! do? Why is it useful in our Collection?**
<br> - Allows you to downcast something as a specific type i.e. if you wanted to make sure you are only depositing a specific NFT into a collection then you forcecast as your token type.

**2. What does auth do? When do we use it?**
<br> - When downcasting referenes you need to use and authorsied reference as denoted with 'auth'. In the example of the NFTs we got a refference to the metadata and downcasted it to our NFT type.

**3. This last quest will be your most difficult yet. Take this contract and add a function called borrowAuthNFT just like we did in the section called "The Problem" above. Then, find a way to make it publically accessible to other people so they can read our NFT's metadata. Then, run a script to display the NFTs metadata for a certain id.**
**You will have to write all the transactions to set up the accounts, mint the NFTs, and then the scripts to read the NFT's metadata. We have done most of this in the chapters up to this point, so you can look for help there :):**
<br> - Contract
``` Cadence
import NonFungibleToken from 0x02

pub contract CryptoPoops: NonFungibleToken {
  pub var totalSupply: UInt64

  pub event ContractInitialized()
  pub event Withdraw(id: UInt64, from: Address?)
  pub event Deposit(id: UInt64, to: Address?)

  pub resource NFT: NonFungibleToken.INFT {
    pub let id: UInt64
    pub let name: String
    pub let favouriteFood: String
    pub let luckyNumber: Int

    init(_name: String, _favouriteFood: String, _luckyNumber: Int) {
      self.id = self.uuid
      self.name = _name
      self.favouriteFood = _favouriteFood
      self.luckyNumber = _luckyNumber
    }
  }

     pub resource interface CollectionPublic {
        pub fun deposit(token: @NonFungibleToken.NFT)
        pub fun getIDs(): [UInt64]
        pub fun borrowNFT(id: UInt64): &NonFungibleToken.NFT
        pub fun borrowAuthNFT(id: UInt64): &NFT
    }

  pub resource Collection: NonFungibleToken.Provider, NonFungibleToken.Receiver, NonFungibleToken.CollectionPublic, CollectionPublic {
    pub var ownedNFTs: @{UInt64: NonFungibleToken.NFT}

    pub fun withdraw(withdrawID: UInt64): @NonFungibleToken.NFT {
      let nft <- self.ownedNFTs.remove(key: withdrawID) 
            ?? panic("This NFT does not exist in this Collection.")
      emit Withdraw(id: nft.id, from: self.owner?.address)
      return <- nft
    }

    pub fun deposit(token: @NonFungibleToken.NFT) {
      let nft <- token as! @NFT
      emit Deposit(id: nft.id, to: self.owner?.address)
      self.ownedNFTs[nft.id] <-! nft
    }

    pub fun getIDs(): [UInt64] {
      return self.ownedNFTs.keys
    }

    pub fun borrowNFT(id: UInt64): &NonFungibleToken.NFT {
      return (&self.ownedNFTs[id] as &NonFungibleToken.NFT?)!
    }

    pub fun borrowAuthNFT(id: UInt64): &NFT {
      let ref = (&self.ownedNFTs[id] as auth &NonFungibleToken.NFT?)!
      return ref as! &NFT
}

    init() {
      self.ownedNFTs <- {}
    }

    destroy() {
      destroy self.ownedNFTs
    }
  }

  pub fun createEmptyCollection(): @NonFungibleToken.Collection {
    return <- create Collection()
  }

  pub resource Minter {

    pub fun createNFT(name: String, favouriteFood: String, luckyNumber: Int): @NFT {
      return <- create NFT(_name: name, _favouriteFood: favouriteFood, _luckyNumber: luckyNumber)
    }

    pub fun createMinter(): @Minter {
      return <- create Minter()
    }

  }

  init() {
    self.totalSupply = 0
    emit ContractInitialized()
    self.account.save(<- create Minter(), to: /storage/Minter)
  }
}
```
<br> - Add Collection
``` Cadence
import CryptoPoops from 0x01
import NonFungibleToken from 0x02

transaction {

  prepare(acct: AuthAccount) {
    acct.save(<- CryptoPoops.createEmptyCollection(), to: /storage/Collection)
    acct.link<&CryptoPoops.Collection{NonFungibleToken.CollectionPublic, CryptoPoops.CollectionPublic}>(/public/Collection, target: /storage/Collection)
  
  }

  execute {
    log("Stored a Collection for our CryptoPoops")
  }
}
```
<br> - Mint and Deposit NFT
``` Cadence
import CryptoPoops from 0x01
import NonFungibleToken from 0x02

transaction(recipient: Address, name: String, favouriteFood: String, luckyNumber: Int) {

  prepare(signer: AuthAccount) {
   let nftMinter = signer.borrow<&CryptoPoops.Minter>(from: /storage/Minter)!

   let publicReference = getAccount(recipient).getCapability(/public/Collection)
                            .borrow<&CryptoPoops.Collection{NonFungibleToken.CollectionPublic}>()
                            ?? panic ("This account does not have a collection")

     let nft <- nftMinter.createNFT(
            name: name,
            favouriteFood: favouriteFood,
            luckyNumber: luckyNumber
        )

    publicReference.deposit(token: <- nft)
    }

    execute {
      log ("NFT minted")
  }
}
```
<br> - Read NFT metadata
``` Cadence
import CryptoPoops from 0x01
import NonFungibleToken from 0x02

pub fun main(address: Address, id: UInt64): UInt64 {
  let publicReference = getAccount(address).getCapability(/public/Collection)
                          .borrow<&CryptoPoops.Collection{CryptoPoops.CollectionPublic}>()
                          ?? panic("This account does not have a Collection")

  let pubref: &CryptoPoops.NFT = publicReference.borrowAuthNFT(id: id)
  
  log(pubref.id)
  log(pubref.name)
  log(pubref.favouriteFood)
  log(pubref.luckyNumber)

  return pubref.id

}
```

