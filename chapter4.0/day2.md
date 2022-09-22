## Quests

**1. What does .link() do?**
<br> - Allows you a way to provide a 'link' from /storage/ to /public/ and /private/.

**2. In your own words (no code), explain how we can use resource interfaces to only expose certain things to the /public/ path.**
<br> - When creating a link you can 'expose' functions or assets, a resource interface is great practise to ensure that the person accessing that path can't see more than you want them to.

**3. Deploy a contract that contains a resource that implements a resource interface. Then, do the following:**
<br>
``` Cadence
pub contract QuestaCon {

pub resource interface Iquest {
    pub var quest: Int
    }

// Quest implements Iquest, a resource interface
pub resource Quest: Iquest {
    pub var quest: Int
    pub var nono: String

    init() {
    self.quest = 240
    self.nono = "You'll never get this"
    }
}

pub fun createQuest():@Quest {
    return <- create Quest()
    }

}

```

**i. In a transaction, save the resource to storage and link it to the public with the restrictive interface.**
<br>
``` Cadence
import QuestaCon from 0x01

transaction() {

  prepare(signer: AuthAccount) {
    var questSave <- QuestaCon.createQuest()
    signer.save(<- questSave, to: /storage/MyQuestResource)

    signer.link<&QuestaCon.Quest{QuestaCon.Iquest}>(/public/MyQuestResource, target: /storage/MyQuestResource)
    
  }

  execute {

  }
}

```

**ii. Run a script that tries to access a non-exposed field in the resource interface, and see the error pop up.**
<br>
``` Cadence
import QuestaCon from 0x01

pub fun main(address: Address): String {
  let publicCapability: Capability<&QuestaCon.Quest{QuestaCon.Iquest}> =
    getAccount(address).getCapability<&QuestaCon.Quest{QuestaCon.Iquest}>(/public/MyQuestResource)

  let testResource: &QuestaCon.Quest{QuestaCon.Iquest} = publicCapability.borrow() ?? panic("The capability doesn't exist or you did not specify the right type when you got the capability.")

  return testResource.nono //member of restricted type is not accessible: nono
}
```

**iii. Run the script and access something you CAN read from. Return it from the script.**
<br>
``` Cadence
import QuestaCon from 0x01

pub fun main(address: Address): Int {
  let publicCapability: Capability<&QuestaCon.Quest{QuestaCon.Iquest}> =
    getAccount(address).getCapability<&QuestaCon.Quest{QuestaCon.Iquest}>(/public/MyQuestResource)

  let testResource: &QuestaCon.Quest{QuestaCon.Iquest} = publicCapability.borrow() ?? panic("The capability doesn't exist or you did not specify the right type when you got the capability.")

  return testResource.quest
}
```
