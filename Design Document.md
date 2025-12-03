# 1. Introduction
  My design implements a player inventory using a HashTable<string, unsigned Int>. The string would be the key and the item name. The unsinged int would be the number of those items the player has.
The downside to this structure is that the elements are not always in a predictable order. This would make it harder to display the inventory in its entirety in a consistent manner.

# 2. Design Philosophy
Hashtables ability to access elements in O(1) time is very desirable. I envision the inventory as a grid system, similar to games like Minecraft, The Legend of Zelda: Breath of the Wild, and Terraria.
This is the format I prefer and find the most sensical. It lays all items in view for the player, and can allow for sorting of items. While this can sometimes make the inventory feel cluttered, it makes
everything visible at once instead of having to scroll or search for waht you are looking for.
The client for this class would be a game that uses an inventory system that allows for duplicates. The user would be someone playing a game that implements that class.

# 3. Core Operations
  ## 1. Display Inventory
  - Displays all items in the inventory in a set grid size
  - Its expected time complexity is O(n^2) As it will first need to retrieve all keys in the hash table, which will take O(n) time. Then it will have to use those keys to get each element the tables, which will again take O(n) time.
  - Possible edge cases include when there are no items in the inventory, and when there are zero of a certain item (which could be good or bad depending exactly how you want to do the inventory system).
  - The Hashtable is good for displaying the operation as it's order can be changed easily, where was with a tree or sequence, the order of the items is linked, and in order to change that order, you would need to use another data structure to temporarily store them in and reorder them. The part that holds the Hashtable back is that it is slower to retrieve all items since it need to first retrieve all the keys, where a tree and sequence would be able to do the same operation in O(n) time.

  ## 2. Search for item
  -  Searches the inventory for a given item, and returns whether or not the item was found, and how many are in the inventory if it was found
  - The expected time complexity is O(n) since it only needs to retrieve the one key
  - Possible edge cases include the item not being found, or there being zero of an item, or multiple entries with the same item. Would need to be worked out if it should only show the item once and combine the amounts, show each 'stack' individually, or something else.
  - Hashtables are great for quickly retrieving a data entry, as they can do so in O(n) time. Having the item name be the key will make searching even easier, since the key to be used in the search can be given by the user. The only constraint would be that the user would have to enter the complete, exact name of the item, which could cause the function to feel to strict. So a way of creating suggestions when typing, or show results that are close to what the user attempted to enter may be necessary.

## 3. Sort inventory
- This would display the inventory in a sorted manner based off some given criteria such as name, amount, type, latest obtained, last used, etc.
- The expected time complexity for this could vary greatly, and would likely require an additional data structure to store the keys in a sorted order. Just displaying the inventory would take O(n) time. Actually sorting the inventory is multi-step process. For example, even if you were choosing to sort based on number of items of a type, you would still need a way to organize them in the case that there are multiple items in the inventory with the same amount, so you would need to sort the smaller set of values that have the same amount by alphabetially or some other way. This would take O(n^2) time in the worst case. Meanwhile a filter such as last used would never need multiple criteria, as time is linear, and so is the order items were used. Something like that could be done in O(n) time, especially if there was a queue keeping track of items being used. Sorting alphabetically would also require a sorting algorithm, but could use a vector.sort() operation, which takes O(n log(n)) time [1].
- Possible edge cases include there being no items in the inventory, no items having ever been used (or wherever the file containing what items were last used cannot be found).
- Hashtables may not be the best for this since they have no set order. However, the keys can be stored in some other data structure, such as a vector or array, and those keys can be sorted. This vector may need to be loaded into memeory at almost all times so that it is ready to quickly pull up alll items in the inventory in the same order, or be rearranged to be sorted. The only benefit to using a hashtable here is that no matter how sorting the keys happens, accessing the items can be done in O(n) time.




# Citations
[1] https://en.cppreference.com/w/cpp/algorithm/sort.html
  
  
