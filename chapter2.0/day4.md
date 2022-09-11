## Quests

**1. Deploy a new contract that has a Struct of your choosing inside of it (must be different than Profile).
<br>- Create a dictionary or array that contains the Struct you defined.
<br>- Create a function to add to that array/dictionary.**

``` cadence

pub contract BestPizzaToppings {

    pub var toppingsRating: {String: Ratings}

    pub struct Ratings {
        pub let name: String
        pub let topping: String
        pub let rating: Int

    init (_name: String, _topping: String, _rating: Int) {
        self.name = _name
        self.topping = _topping
        self.rating = _rating
    }

}

    pub fun addRating (name: String, topping: String, rating: Int) {
        self.toppingsRating[name] = Ratings(_name: name, _topping: topping, _rating: rating)

}

init(){
    self.toppingsRating = {}
}  

}
```


**2. Add a transaction to call that function in step 3.**

```cadence
import BestPizzaToppings from 0x01

transaction(name: String, topping: String, rating: Int) {

    prepare(signer: AuthAccount) {}

    execute {
        BestPizzaToppings.addRating(name: name, topping: topping, rating:rating)
    }

}
```

**3. Add a script to read the Struct you defined.**

``` cadence
import BestPizzaToppings from 0x01

pub fun main(name: String): BestPizzaToppings.Ratings {
    return BestPizzaToppings.toppingsRating[name]!

}
```
