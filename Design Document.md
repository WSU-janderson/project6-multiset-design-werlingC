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
  > Displays all items in the inventory in a set grid size

  > Its expected time complexity is O(n^2) As it will first need to retrieve all keys in the hash table, which will take O(n) time. Then it will have to use those keys to get each element the tables, which will again take O(n) time.

  > Possible edge cases include when there are no items in the inventory, and when there are zero of a certain item (which could be good or bad depending exactly how you want to do the inventory system).

  > The Hashtable is good for displaying the operation as it's order can be changed easily, where was with a tree or sequence, the order of the items is linked, and in order to change that order, you would need to use another data structure to temporarily store them in and reorder them. The part that holds the Hashtable back is that it is slower to retrieve all items since it need to first retrieve all the keys, where a tree and sequence would be able to do the same operation in O(n) time.

  ## 2. Search for item
  > Searches the inventory for a given item, and returns whether or not the item was found, and how many are in the inventory if it was found

  > The expected time complexity is O(n) since it only needs to retrieve the one key

  > Possible edge cases include the item not being found, or there being zero of an item, or multiple entries with the same item
  
  
