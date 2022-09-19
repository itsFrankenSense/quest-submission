## Quests

**1. Define your own contract that stores a dictionary of resources. Add a function to get a reference to one of the resources in the dictionary.** 
<br>
``` Cadence
pub contract ReferenceItem {

    pub var dictionaryofcar: @{String: Car}

    pub resource Car {
        pub let model: String
        init(_model: String) { 
        self.model = _model
        }
    }

     pub fun refferenceofcar(key: String): &Car {
        return (&self.dictionaryofcar[key] as &Car?)!
    }
    
    init(){
        self.dictionaryofcar <- {
        "KE70": <- create Car(_model: "1990"),
        "KE30": <- create Car(_model: "1987")
        }
    }

}

```


**2. Create a script that reads information from that resource using the reference from the function you defined in part 1.**
<br>
``` Cadence

import ReferenceItem from 0x01

pub fun main(): String {
    let ref = ReferenceItem.refferenceofcar(key: "KE70")
    return ref.model

}
```

**3. Explain, in your own words, why references can be useful in Cadence.**
<br> - Refferences allow you to access (typically) a Struct or Resource, without having to move the information around to access it.
<br> With a refernce you don't have to move the resource out of storage just to view the item.
