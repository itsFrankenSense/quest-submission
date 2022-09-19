## Quests

**1. Explain, in your own words, the 2 things resource interfaces can be used for (we went over both in today's content)**
<br> - You can use resource interfaces to chose what information inside a resource/struct is visible, and to what people. 
<br> - It also confirms that resource has that variable in it as well.


**2. Define your own contract. Make your own resource interface and a resource that implements the interface. Create 2 functions. In the 1st function, show an example of not restricting the type of the resource and accessing its content. In the 2nd function, show an example of restricting the type of the resource and NOT being able to access its content.**
<br>
``` cadence
access(all) contract FrankenSense {

    //this resource interface has a variable inside it called 'game0', which is a string
    pub resource interface IVideoGames {
        pub let game0: String
    }

    // : used to implement the interface with the 'VideoGames' resource
    pub resource VideoGames: IVideoGames {
        pub let game0: String
        pub let game1: String

        init () {
        self.game0 = "Valorant"
        self.game1 = "PubG"
        }
    }

    // {} = after the resouce restricts access to the resource interface
    pub fun noAccess(): @VideoGames{IVideoGames} {
        return <- create VideoGames()
    }
    // as this does not have {} it allows access to the resource
    pub fun yesAccess(): @VideoGames {
        return <- create VideoGames()
    }

init() {
}

}
```

**3. How would we fix this code?**
<br> - Before
``` cadence
pub contract Stuff {

    pub struct interface ITest {
      pub var greeting: String
      pub var favouriteFruit: String
    }

    // ERROR:
    // `structure Stuff.Test does not conform 
    // to structure interface Stuff.ITest`
    pub struct Test: ITest {
      pub var greeting: String

      pub fun changeGreeting(newGreeting: String): String {
        self.greeting = newGreeting
        return self.greeting // returns the new greeting
      }

      init() {
        self.greeting = "Hello!"
      }
    }

    pub fun fixThis() {
      let test: Test{ITest} = Test()
      let newGreeting = test.changeGreeting(newGreeting: "Bonjour!") // ERROR HERE: `member of restricted type is not accessible: changeGreeting`
      log(newGreeting)
    }
}
```

<br> - After
``` cadence
pub contract Stuff {

// added "changeGreeting"
    pub struct interface ITest {
      pub var greeting: String
      pub var favouriteFruit: String
      pub fun changeGreeting(newGreeting: String): String
    }

// added "favouriteFruit"
    pub struct Test: ITest {
      pub var greeting: String
      pub var favouriteFruit: String

      pub fun changeGreeting(newGreeting: String): String {
        self.greeting = newGreeting
        return self.greeting
      }

      init() {
        self.greeting = "Hello!"
        self.favouriteFruit = "Apple"
      }
    }

    pub fun fixThis() {
      let test: Test{ITest} = Test()
      let newGreeting = test.changeGreeting(newGreeting: "Bonjour!")
      log(newGreeting)
    }
}
```


