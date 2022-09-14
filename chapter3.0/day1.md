## Quests

**1. In words, list 3 reasons why structs are different from resources.**
<br> - They can not be copied
<br> - Must be moved or destroyed (can't be left in a function)
<br> - They are much harder to deal with due to their strict nature

**2. Describe a situation where a resource might be better to use than a struct.**
<br> - When handling sensitive information that needs to be handled with care. i.e. Important documents, and NFT's.

**3. What is the keyword to make a new resource?**
<br>``` create ```

**4. Can a resource be created in a script or transaction (assuming there isn't a public function to create one)?**
<br> Nope, has to be done in the contract.

**5. What is the type of the resource below?**
<br> - @Jacob

``` cadence
pub resource Jacob {

}
```
**5. Let's play the "I Spy" game from when we were kids. I Spy 4 things wrong with this code. Please fix them.**

``` cadence
pub contract Test {

    // Hint: There's nothing wrong here ;)
    pub resource Jacob {
        pub let rocks: Bool
        init() {
            self.rocks = true
        }
    }

    pub fun createJacob(): Jacob { // there is 1 here
        let myJacob = Jacob() // there are 2 here
        return myJacob // there is 1 here
    }
}
```

**Revised code**

``` cadence
pub contract Test {

    // Hint: There's nothing wrong here ;)
    pub resource Jacob {
        pub let rocks: Bool
        init() {
            self.rocks = true
        }
    }

    pub fun createJacob(): @Jacob { // added '@' to define the return resource type
        let myJacob <- create Jacob() // added the move operator '<-' and 'create'
        return <- myJacob // added the move operator '<-'
    }
}
```
