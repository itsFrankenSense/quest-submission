## Quests

**1. In a script, initialize an array (that has length == 3) of your favourite people, represented as Strings, and log it.**
```
pub fun main() {

var frieendsss: [String] = ["Ellie", "Bear", "Jacobs arch nemesis"]
    log(frieendsss)
    log(frieendsss.length)

}
```
Return line 1: ["Ellie", "Bear", "Jacobs arch nemesis"]
<br>Return line 2: 3
<br>
<br> **2. In a script, initialize a dictionary that maps the Strings Facebook, Instagram, Twitter, YouTube, Reddit, and LinkedIn to a UInt64 that represents the order in which you use them from most to least. For example, YouTube --> 1, Reddit --> 2, etc. If you've never used one before, map it to 0!**
```
pub fun main() {

var socialMedia: {String:UInt64} = {"Facebook": 2, "Instagram": 3, "Twitter": 4, "YouTube": 5, "Reddit": 1, "LinkedIn": 0}
    log (socialMedia.containsKey("Facebook"))

}
```
Just added a bool log for fun.
<br>Return line 1 : true 
<br>
<br> **3. Explain what the force unwrap operator ! does, with an example different from the one I showed you (you can just change the type).**
<br> As we are expecting to return nil or an optional, it must be unwrapped. If nil we will panic, otherwise it will return that specific type.

```
pub fun main():UInt64? {

var socialMedia: {String:UInt64} = {"Facebook": 2, "Instagram": 3, "Twitter": 4, "YouTube": 5, "Reddit": 1, "LinkedIn": 0}
    return socialMedia["Facebook"]!

}
```
<br>Return line 1 : {"type":"Optional","value":{"type":"UInt64","value":"2"}}
<br>
<br> **Using this picture below, explain...**
<img width="1361" alt="wrongcode" src="https://user-images.githubusercontent.com/111278229/189252019-9696fc38-da84-428c-9110-036184ca05bf.png">
<br> **- What the error message means**
<br> **- Why we're getting this error**
<br> **- How to fix it**
<br> The expected return type was as a String when it should be an optional String - 'String?' Also then needing a force unwrap on the return 'return thing[0x03]!'
