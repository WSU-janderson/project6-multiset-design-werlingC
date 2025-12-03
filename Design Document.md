# 1. Introduction
  My design implements a player inventory using a HashTable<string, unsigned Int>. The string would be the key and the item name. The unsinged int would be the number of those items the player has.
The downside to this structure is that the elements are not always in a predictable order. This would make it harder to display the inventory in its entirety in a consistent manner.

# 2. Design Philosophy
Hashtables ability to access elements in O(1) time is very desirable. I envision the inventory as a grid system, similar to games like Minecraft, The Legend of Zelda: Breath of the Wild, and Terraria.
This is the format I prefer and find the most sensical. It lays all items in view for the player, and can allow for sorting of items. While this can sometimes make the inventory feel cluttered, it makes
everything visible at once instead of having to scroll or search for waht you are looking for.
The client for this class would be a game that uses an inventory system that allows for duplicates. The user would be someone playing a game that implements that class.

# 3. Core Operations
### 1. Display Inventory
  - Displays all items in the inventory in a set grid size
  - Its expected time complexity is O(n^2) As it will first need to retrieve all keys in the hash table, which will take O(n) time. Then it will have to use those keys to get each element the tables, which will again take O(n) time.
  - Possible edge cases include when there are no items in the inventory, and when there are zero of a certain item (which could be good or bad depending exactly how you want to do the inventory system).
  - The Hashtable is good for displaying the operation as it's order can be changed easily, where was with a tree or sequence, the order of the items is linked, and in order to change that order, you would need to use another data structure to temporarily store them in and reorder them. The part that holds the Hashtable back is that it is slower to retrieve all items since it need to first retrieve all the keys, where a tree and sequence would be able to do the same operation in O(n) time.

### 2. Search for item
  -  Searches the inventory for a given item, and returns whether or not the item was found, and how many are in the inventory if it was found
  - The expected time complexity is O(n) since it only needs to retrieve the one key
  - Possible edge cases include the item not being found, or there being zero of an item, or multiple entries with the same item. Would need to be worked out if it should only show the item once and combine the amounts, show each 'stack' individually, or something else.
  - Hashtables are great for quickly retrieving a data entry, as they can do so in O(n) time. Having the item name be the key will make searching even easier, since the key to be used in the search can be given by the user. The only constraint would be that the user would have to enter the complete, exact name of the item, which could cause the function to feel to strict. So a way of creating suggestions when typing, or show results that are close to what the user attempted to enter may be necessary.

### 3. Sort inventory
- This would display the inventory in a sorted manner based off some given criteria such as name, amount, type, latest obtained, last used, etc.
- The expected time complexity for this could vary greatly, and would likely require an additional data structure to store the keys in a sorted order. Just displaying the inventory would take O(n) time. Actually sorting the inventory is multi-step process. For example, even if you were choosing to sort based on number of items of a type, you would still need a way to organize them in the case that there are multiple items in the inventory with the same amount, so you would need to sort the smaller set of values that have the same amount by alphabetially or some other way. This would take O(n^2) time in the worst case. Meanwhile a filter such as last used would never need multiple criteria, as time is linear, and so is the order items were used. Something like that could be done in O(n) time, especially if there was a queue keeping track of items being used. Sorting alphabetically would also require a sorting algorithm, but could use a vector.sort() operation, which takes O(n log(n)) time [1].
- Possible edge cases include there being no items in the inventory, no items having ever been used (or wherever the file containing what items were last used cannot be found).
- Hashtables may not be the best for this since they have no set order. However, the keys can be stored in some other data structure, such as a vector or array, and those keys can be sorted. This vector may need to be loaded into memeory at almost all times so that it is ready to quickly pull up alll items in the inventory in the same order, or be rearranged to be sorted. The only benefit to using a hashtable here is that no matter how sorting the keys happens, accessing the items can be done in O(n) time.

### 4. Increment Item
- This would would increment/decrement and item by a given value. Giving a public method to do this would ensure that any cases where the number of items is less than 1 is accounted for and is properly removed from the inventory.
- The expected time complexity is O(1) since Hashtables allow for direct access.
- Possible edge cases include an item being deincremented to be less than 1 (in which case they should be removed). Another edge case is when a value is increased beyond what an unsigned int can hold an overflow may occur, so there needs to be a value cap. This cap would likely depend on item type, similar to Minecraft, where some items cannot be 'stacked' while others can be stacked up to 64, and some can be stacked to only 16. Same thing applies when the value would be taken below zero. The number of commands needs to be checked, and possibly return a boolean value to let whatever called the function know that either know more items can be added/removed.
- A Hashtable would work well for this, as it is able to directly access each element and modify its value. There are no real downsides to using a hashtable for this function.

### 5. Add item
- This would add an item to the inventory as either an entirely new entry, or a duplicate of another item with its own value count. The default value would be 1, but it could be specified to be any unsigned int.
- The expected time complexity would be O(1) time, as values can be added directly and aren't linked to other entries. However, since duplicates are allowed there needs to be a check if there is an existing key with the same name, which would take O(n) to check all keys.
- Possible edge cases include the same overflow and undeflow type problems as discussed in the Increment Item function. In addition, there is the case where a key is already in the table, since duplicates are allowed, there would need to be a way to distinctly reference each pair with a key, and also have the search function bring up all key-value pairs of that item
- Hashtables are again nice for their ability to add a new key-value pair in O(1) time. However, the item name being the key causes some problems when it comes to duplicates. A number could be added to the end of the key name signifying that it is a distinct value. As discussed in the search function, having a way to return not just the exact match, but also close matches would also return these keys with 1 value added to them.

# 4. Set Operations
### Loot all (union_with())
  This function would allow a player to automatically take all items from a chest or other container and add it to their inventory. The proper way to do this is to search if an existing stack (key-value pair) exists, and add the items from the container to that stack. If the number of items in the stack exceeds its limit (or rather, if it would), then it should create a new stack and add the remainder to that. It should place that new key in the key vector immediately after the original stack it added to. For items that don't having a matching item already in the inventory they would be added as normal, creating new key-value pairs to the hashtable, adding the keys to the end of the keys vector. The complexity would be O(n^2) for the whole operation. As for each item in the container, it would need to check if that item exists in the inventory already, and then either increment a value, or create a new key-value pair.
  Possible edge cases included the scenario where the inventory is filled. Or when there is a duplicate stack. With my implementation, duplicate stacks would have a number added to the item name to make the key unique. So instead of comparing the keys directly, they would need to compare the strings, and check that the strings at least start with the same characters, and the duplicate stack would be immediately followed by number characters (to ensure cases like **potion of** healing does not get confused with **potion of** swiftness). When the inventory is full, the function should immediately end, and all remaining items should not be added, which is both excpected and easy to implement.

### Necesscary Items (intersection_with())
  This function would be used for making sure that a player has all the necessary components in their inventory for some action such as crafting, trading, or giving them for a quest. a crafting recipe or trade request would be formatted as new inventory that has all the items and their values needed for that action. It would then check the player's inventory to ensure that all those items and a minimum of that value are present to perform that action. After the check, the transaction would be completed, and it would remove those items from the inventory, and add the crafted item, traded item, or gold to the players inventory. This would be done with the Add item function.


# Citations
[1] https://en.cppreference.com/w/cpp/algorithm/sort.html
  
  
