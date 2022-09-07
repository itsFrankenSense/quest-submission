## Quests

**1. Explain why we wouldn't call changeGreeting in a script.**
<br>A script is used to view information, not change it.

**2. What does the AuthAccount mean in the prepare phase of the transaction?**
<br>AuthAccount is used to access assets stored in the users storage.

**3. What is the difference between the prepare phase and the execute phase in the transaction?**
<br>Prepare passes in the access, where as execute completes the functions. Technically all can be done from prepare, it just looks nicer this way.

**4. Add two new things inside your contract:**

- A variable named myNumber that has type Int (set it to 0 when the contract is deployed)
- A function named updateMyNumber that takes in a new number named newNumber as a parameter that has type Int and updates myNumber to be newNumber

```
pub contract HelloWorld {

    pub var greeting: String
    pub var myNumber: Int

    pub fun changeGreeting(newGreeting: String) {
        self.greeting = newGreeting
    }

    pub fun updateMyNumber(newNumber: Int) {
        self.myNumber = newNumber
    }

    init() {
        self.greeting = "Hello, World!"
        self.myNumber = 0
    }
}
```

- Add a script that reads myNumber from the contract

```
import HelloWorld from 0x01

pub fun main(): Int {
    return HelloWorld.myNumber
}
```

- Add a transaction that takes in a parameter named myNewNumber and passes it into the updateMyNumber function. Verify that your number changed by running the script again.

```
import HelloWorld from 0x01

transaction(myNewNumber: Int) {

    prepare(signer: AuthAccount) {}

    execute {
        HelloWorld.updateMyNumber(newNumber: myNewNumber)
    }

}
```
<img width="517" alt="2 2 4" src="https://user-images.githubusercontent.com/111278229/188804078-47b33ac6-d4d0-4c8a-a494-bed119b06860.png">

