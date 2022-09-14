## Quests

1. Write your own smart contract that contains two state variables: an array of resources, and a dictionary of resources. Add functions to remove and add to each of them. They must be different from the examples above.
<br>

``` cadence

pub contract Deets {

    pub var dictionaryOfdetails: @{Int: DetailsD}
    pub var arraryOfdetails: @[DetailsA]

    pub resource DetailsD {
        pub let number: Int
        init() {
            self.number = 0423000000
        }
    }

    pub resource DetailsA {
        pub let postcode: Int
        init() {
            self.postcode = 7000
        }
    }

    pub fun addDetails(deets: @DetailsD) {
        let key = deets.number
        self.dictionaryOfdetails[key] <-! deets
    }

        pub fun removeDetails(key: Int): @DetailsD {
        let details <- self.dictionaryOfdetails.remove(key: key) ?? panic("Could not find the details!")
        return <- details
    }

    pub fun addDetailsD(postcode: @DetailsA) {
        let key = postcode.postcode
        self.arraryOfdetails.append(<- postcode)
    }
        pub fun removeDetailsD(key: Int): @DetailsA {
        let details <- self.arraryOfdetails.remove(at: key)
        return <- details
    }

    init() {
        self.dictionaryOfdetails <- {}
        self.arraryOfdetails <- []
    }

}

```

