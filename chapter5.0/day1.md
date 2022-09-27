## Quests

**1. Describe what an event is, and why it might be useful to a client.**
<br> - An event emitted to the network is a way for us to see what happened during the transaction. Contract owners can choose what to emit during these transactions.
Emitting events is great for things like marketplaces, and analytics.

**2. Deploy a contract with an event in it, and emit the event somewhere else in the contract indicating that it happened.**
<br>
``` Cadence
pub contract Burger {

    pub event WhatsInside(id: String)
    pub var salad: String

    pub fun burgers(): String {
        return self.salad
    }

init() {
        self.salad = "Tomato"
        emit WhatsInside(id: self.salad)
    }

}
```

**i. Using the contract in step 2), add some pre conditions and post conditions to your contract to get used to writing them out.**
<br>
``` Cadence
pub contract Burger {

    pub event WhatsInside(id: String)
    pub var salad: String

    pub fun burgers(): String {
        post {
          result == self.salad: "The returned salad is not the same as our state"

        }
        return self.salad
    }

    pub fun changeSalad(newSalad: String){
        pre {
            newSalad != "Cheese": "I'm lactose intolerant"
        }
        self.salad = newSalad
    
    }

init() {
        self.salad = "Tomato"
        emit WhatsInside(id: self.salad)
    }

}
```

**3. For each of the functions below (numberOne, numberTwo, numberThree), follow the instructions.**
<br> - Answers on lines - "//ANSWER:..."
``` Cadence
pub contract Test {

  // TODO
  // Tell me whether or not this function will log the name.
  // name: 'Jacob'

  //ANSWER: Yes it will log the name
  //       'Jacob' is equal to 5 characters. 
  pub fun numberOne(name: String) {
    pre {
      name.length == 5: "This name is not cool enough."
    }
    log(name)
  }

  // TODO
  // Tell me whether or not this function will return a value.
  // name: 'Jacob'

  //ANSWER: Yes it will return a value
  //        Pre condition is that the name be longer than 0 which is TRUE. The post condition requires the result to be 'Jacob Tucker' after concatenated which is also TRUE.
  pub fun numberTwo(name: String): String {
    pre {
      name.length >= 0: "You must input a valid name."
    }
    post {
      result == "Jacob Tucker"
    }
    return name.concat(" Tucker")
  }

  pub resource TestResource {
    pub var number: Int

    // TODO
    // Tell me whether or not this function will log the updated number.
    // Also, tell me the value of `self.number` after it's run.

    //ANSWER: Yes this function will log the updated number of 1
    //        Post condition requires the new number to be the same as the old number + 1 which is TRUE
    pub fun numberThree(): Int {
      post {
        before(self.number) == result + 1
      }
      self.number = self.number + 1
      return self.number
    }

    init() {
      self.number = 0
    }

  }

}
```
