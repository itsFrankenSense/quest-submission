## Quests

**1. Why did we add a Collection to this contract? List the two main reasons.**
<br> - If you tried to create another NFT (without a collection) it would say that something is already stored at that address. It also provides a location for others to deposit NFTs into your account at this path.

**2. What do you have to do if you have resources "nested" inside of another resource? ("Nested resources")**
<br> - 1. When moving you have to use the .remove mothod and specify the key.
<br> - 2. A nested resource also requires a destroy function that can be called.

**3. Brainstorm some extra things we may want to add to this contract. Think about what might be problematic with this contract and how we could fix it.**
<br> - Metadata, whiteList conditions, pricing conditions, wallet limits.

**i. Idea #1: Do we really want everyone to be able to mint an NFT? 🤔.**
<br> - No, adding a pre condition for certain wallets to access the mint function.

**ii. Idea #2: If we want to read information about our NFTs inside our Collection, right now we have to take it out of the Collection to do so. Is this good?**
<br> - No, you would want to add a way to 'borrow' a refference inside a resource interface.
