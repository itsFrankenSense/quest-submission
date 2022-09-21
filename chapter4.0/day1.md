## Quests

**1. Explain what lives inside of an account**
<br> - Contract Code & Account storage (/storage)

**2. What is the difference between the /storage/, /public/, and /private/ paths?**
<br> - /storage is where everything actually lives. /public and /private are just 'links' to what is in /storage and can help with restriciting who can access what.

**3. What does .save() do? What does .load() do? What does .borrow() do?**
<br> - .save() saves a resource into account storage
<br> - .load() takes a resource out of account storage
<br> - .borrow() borrows a refference to an item in account storage

**4. Explain why we couldn't save something to our account storage inside of a script.**
<br> - This is modifiying data so it needs to be a transaction

**5. Explain why I couldn't save something to your account.**
<br> - You would need "Auth Account" access, otherwise you can't.

**6 Define a contract that returns a resource that has at least 1 field in it. Then, write 2 transactions:**
``` Cadence
pub contract QuestaCon {

pub resource Quest {
    pub var quest: Int
    init() {
    self.quest = 240
    }
}

pub fun createQuest():@Quest {
    return <- create Quest()
    }

}
```

**i. A transaction that first saves the resource to account storage, then loads it out of account storage, logs a field inside the resource, and destroys it.**
``` Cadence 
import QuestaCon from 0x01

transaction() {
  prepare(signer: AuthAccount) {

  // moving whatver is the function of .createQuest into the let questSave.
    let questSave <- QuestaCon.createQuest()

  // then we move that into storeage at /storage/MyQuestResource
    signer.save(<- questSave, to: /storage/MyQuestResource)

  //now we are attempting to whatever is at the path /storage/MyQuestResource but we are telling the playground we expect there is something in <@QuestaCon.Quest> (the resource we stored)
    let questLoad <- signer.load<@QuestaCon.Quest>(from: /storage/MyQuestResource)
    ?? panic("No resource for you")
    log (questLoad.quest)

    destroy questLoad

  }

  execute {

  }
}
```

**ii. A transaction that first saves the resource to account storage, then borrows a reference to it, and logs a field inside the resource.**

``` Cadence
import QuestaCon from 0x01

transaction() {
  prepare(signer: AuthAccount) {
    let questSave <- QuestaCon.createQuest()
    signer.save(<- questSave, to: /storage/MyQuestResource)

  //chaging from Load to borrow was a matter of changing the signer.load to borrow. Removing the <-, and destroy as we are not moving the resource. updated variable names as a nice thing.
    let questBorrow = signer.borrow<&QuestaCon.Quest>(from: /storage/MyQuestResource)
    ?? panic("No resource for you")
    log (questBorrow.quest)

  }

  execute {

  }
}
```
