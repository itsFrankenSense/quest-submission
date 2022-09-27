## Quests

**1. Because we had a LOT to talk about during this Chapter, I want you to do the following:
Take our NFT contract so far and add comments to every single resource or function explaining what it's doing in your own words. Something like this:**

```Cadence
pub contract CryptoPoops {

  // This is a total supply tracker (initialised to 0 at the bottom of the contract)
  pub var totalSupply: UInt64

  // This is an NFT resource that contains a name, favouriteFood, and luckyNumber
  pub resource NFT {
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

  // This is a resource interface allows the calling of the 'deposit', 'getID' and 'borrowNFT' functions.
  pub resource interface CollectionPublic {
    pub fun deposit(token: @NFT)
    pub fun getIDs(): [UInt64]
    pub fun borrowNFT(id: UInt64): &NFT
  }

  // This is our collection resource that maps a UInt64 to a 'NFT' type
  pub resource Collection: CollectionPublic {
    pub var ownedNFTs: @{UInt64: NFT}

    // force move the token to that token id location.
    pub fun deposit(token: @NFT) {
      self.ownedNFTs[token.id] <-! token
    }

// this function is not publicly callable as it is not in the resource interface.
    pub fun withdraw(withdrawID: UInt64): @NFT {
      let nft <- self.ownedNFTs.remove(key: withdrawID) 
              ?? panic("This NFT does not exist in this Collection.")
      return <- nft
    }

    // the 'getIDs' function is used to return the ID's owned. Returning a UInt64 mapped at the keys.
    pub fun getIDs(): [UInt64] {
      return self.ownedNFTs.keys
    }

    // the 'borrowNFT' function is used to reference an NFT resource at a set ID.
    pub fun borrowNFT(id: UInt64): &NFT {
      return (&self.ownedNFTs[id] as &NFT?)!
    }

    init() {
      self.ownedNFTs <- {}
    }

    // nested resources must have a destroy function
    destroy() {
      destroy self.ownedNFTs
    }
  }

  // this function creates an empty collection and returns it to @Collection
  pub fun createEmptyCollection(): @Collection {
    return <- create Collection()
  }

  // this is our minter resource which is initialised into the account when this contract is deployed.
  pub resource Minter {

    // // the 'createNFT' function is used to create an NFT with 'name', 'favouriteFood' and 'luckyNumber' stored.
    pub fun createNFT(name: String, favouriteFood: String, luckyNumber: Int): @NFT {
      return <- create NFT(_name: name, _favouriteFood: favouriteFood, _luckyNumber: luckyNumber)
    }

    // function to store 'Minter' resource and return '@Minter'
    pub fun createMinter(): @Minter {
      return <- create Minter()
    }

  }
// total supply set to 0
// the minter is saved to the account storage so the depolying account is acting as the 'admin' of the minter function.
  init() {
    self.totalSupply = 0
    self.account.save(<- create Minter(), to: /storage/Minter)
  }
}
```
