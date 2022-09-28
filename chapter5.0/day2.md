## Quests

**1. Explain why standards can be beneficial to the Flow ecosystem.**
<br> - Standards ensure that the product can be adapted and used by mulitple systems / services.
i.e. building a game or a marketplace that wants to integrate NFTs, if they all abide by the same standard this integration will be much easier.

**2. What is YOUR favourite food?**
<br> - Urghhh that's a tough one. I suppose it's probably pizza. I'm basic

**3. Please fix this code (Hint: There are two things wrong):**

The contract interface (not modified):
``` Cadence
pub contract interface ITest {
  pub var number: Int
  
  pub fun updateNumber(newNumber: Int) {
    pre {
      newNumber >= 0: "We don't like negative numbers for some reason. We're mean."
    }
    post {
      self.number == newNumber: "Didn't update the number to be the new number."
    }
  }

  pub resource interface IStuff {
    pub var favouriteActivity: String
  }

  pub resource Stuff {
    pub var favouriteActivity: String
  }
}
```

The implementing contract:
``` Cadence

// added this line in to import the contract interface.
import ITest from 0x01

// added ':ITest' to implement the interface
pub contract Test:ITest {
  pub var number: Int
  
  pub fun updateNumber(newNumber: Int) {
    self.number = 5
  }

// added 'ITest' as it does not know where 'IStuff' is otherwise.
   pub resource Stuff: ITest.IStuff {
    pub var favouriteActivity: String
  }

  pub resource Stuff: IStuff {
    pub var favouriteActivity: String

    init() {
      self.favouriteActivity = "Playing League of Legends."
    }
  }

  init() {
    self.number = 0
  }
}
```
