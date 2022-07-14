# Hashing
## Objectives
* The main idea of hashing techniques
* Different has functions
* Collision resolutions for contiguous implementation 
* Collision resolution for linked implementation
* Analysis of hashing
## Introduction
* Hashing is usually faster for searching than all the algorithms we learned so far
  - <img width="448" alt="Screen Shot 2022-07-12 at 5 47 17 PM" src="https://user-images.githubusercontent.com/89602311/178611150-515456b6-5da2-4f72-ac27-2844c2accd13.png">
* Basic idea: using a one-to-one correspondent hash function hash() to index the input
  - Convert an alphabetic key into an integer
  - Using this integer as an index for an array
  - Check the existence of this alphabetic key in O(1). (Why this can be O(1)?)
* One problem
  - The number of possible keys could be very big
    - Plate number with 7 characters: 36^7 ~= 7.8 * 10^10 (146 GB)
  - The large number of possible keys makes it impossible to design a real one-to-one hash function
    - It has to be many-to-one, i.e., several keys correspond to one location in the array
  - <img width="594" alt="Screen Shot 2022-07-14 at 3 58 49 PM" src="https://user-images.githubusercontent.com/89602311/179084905-cca1cc8d-fcdf-48d8-bd2f-494d2c6a8200.png">
* Hash table
  - The idea of hashing actually allows multiple keys to be mapped to the same location (index) in the array/list/table
    - The array is called a hash table
    - It is possible that two records are mapped into the same place: a collision
  - A good hash function may satisfy:
    - Easy to compute
      - The running time of hash function getting an index of the hash table must be O(1)
    - Avoid collisions
      - Avoid mapping two keys to the same index
    - spread keys evenly distributed in the table
  - Issues
    - The keys are usually not predictable
    - Otherwise an universal hash function may be developed
* Compare two functions (with collisions): 
  - <img width="733" alt="Screen Shot 2022-07-14 at 4 04 03 PM" src="https://user-images.githubusercontent.com/89602311/179085752-ad9ca694-f8a7-43ca-b699-9b4487b6063f.png">
* The hash function usually:
  - takes the key
  - chops it up
  - mixes the pieces together in various ways, and
  - obtains an index that (hopefully) will be uniformly distributed
* Common methods for building a hash function
  - Trunication
    - E.g., the hash function may pick the 1st, 2nd, and 5th digits from teh right side of the key to produce the index, e.g., the key 21296876 can be mapped to 976
  - Folding 
    - E.g., the hash function may divide an integer key into groups of three, three, and two digits, e.g., 21296876 maps to 212 + 968 + 776 = 1256
  - Modular arithmetic
    - E.g., horner(string s): ((31 * horner) + (int)s[i) % M
    - The best choice for modulus is often, but not always, a prime number, which usually spreads the keys quite uniformly
* Common procedures
  - Initialization: create an empty array/list/table
  - Insertion
    - Insert a new key into an empty location: mapping by hash()
      - If duplicated, do not insert
      - If collision occurs (two records are mapped to the same location), we need to resolve the collision
  - Retrieval / Deletion
    - Find the location by mapping with hash()
    - If it's the desired record: return the information
      - Otherwise, use collision resolution
      - If still can't find a match: record is not in the table
* Performance
  - There is no absolutely / univerally good hash function
  - Depend on the application, one hash function may be better than another
    - Empirical studies are often needed to find a good hash function for a particular application
## Collision Resolution
* A collision arises when we have two keys that hash in the same array entry, i.e. h(k1) = h(k2) for k1 != k2
* There are two ways to resolve collision:
  - Hashing with open addressing:
    - If a new key hashes to a table entry which is already occupied, systematically examine other entries until it finds one empty entry to place the new key
      - E.g., linear probing, quadratic probing, etc.
  - Hashing with Chaining:
    - Every hash table entry contains a pointer to a linked list of keys that hash into the same entry
      - E.g., an array of linked lists
* <img width="693" alt="Screen Shot 2022-07-14 at 4 17 41 PM" src="https://user-images.githubusercontent.com/89602311/179087768-f3193a6f-d706-4fd3-8186-4959a212cf7f.png">
* General idea:
  - Find a "protocol" to insert the new element
  - Follow the same "protocol" to find the element for search and delete operation
  - We wish to design an increment function in addition to the hashing function, so that we can find and arrange a new location for the new element with minimal number of probes
    - probe: Looking at one entry and comparing its key with the target
* We will focus on Open Addressing
  - Linear probing
  - Quadratic probing
  - Rehasing: Resizing the hash table
  - double hashing
## Linear Probing
* Start with the hash address (the location where the collision occured) and do a sequential search through the table for the desired key or an empty location
  - The increment function is i
* The table should be considered circular (but just one trip)
  - When the last location is reached, the search proceeds to the first location of the table
* Due to sequential search, the linear probing guarantees finding  an empty slot if the table is not completely full
* Example:
  - Keys: 13, 32, 77, 5, 57, 25, 38, 52
  - Hash table size: 13
  - Hash function: key % 13
  - Number of collision: 4
  - <img width="713" alt="Screen Shot 2022-07-14 at 4 23 26 PM" src="https://user-images.githubusercontent.com/89602311/179088554-51f4233c-62b8-47cc-9ede-30a5bfa2937c.png">
## Quadratic Probing
* Probe pattern in quadratic probing:
  - If there is a collison at hash address h, quadratic probing probes the table at locations h + i^2, i.e., h+1, h+4, h+9,....
    - The increment function is i^2
  - Example: 
    - Keys: 13, 32, 77, 5, 57, 25, 38, 52
    - Hash table size: 13
    - Hash function: key % 13
    - Number of collision: 4
      - E.g., to add 25, first try position 12, then 12+1, 12+4
    - <img width="663" alt="Screen Shot 2022-07-14 at 4 25 56 PM" src="https://user-images.githubusercontent.com/89602311/179088910-adf65d77-65ec-4253-97f4-445913f9abbe.png">
  - Quadratic probing substantially reduces clustering
  - But it does not probe all locations in the hash function
    - When hash_size is a large power of 2, approximately 1/6 of the positions are probed
    - When hash_size is a prime number, quadratic probing can reach 1/2 the locations in the array
      - The resilts are quite satisfactory for prime table/hash sizes
      - Theorem: If a quadratic probing is used and the hash table is prime, a new element can always be inserted if the load factor is (lambda symbol) < 0.5
      - The load factor, (lambda symbol), of a hash table is the ratio of the number of elements (or keys) in the hash table to the table size
    - Overflow may be reported if the load factor reaches 0.5
      - Then we can resize the hashtable (rehashing)
## Resizing the Hash Table
* We don't know how many inserts/deletes are there
  - We can intialize it with a primze size? E.g. hash_size = 997
* If the table is too full: search, insert, delete will be slow
  - We will increase the size of the hash table (e.g., if (lambda symbol) > 0.5)
* If the load is too low: waste of memory (do it optionally)
  - We will decrease the size of the hash table (e.g., if (lambda symbol) < 0.25)
* Resizing a hash table consists of 
  - Creating a new hash table of a new size
  - Choosing a new hash function
  - Iterating through the elements of the old table, and inserting them into the new table
* Example:
  - Keys: 14, 19. 5, 26, 63, 8
  - Initial hash table size: 7 (in real case, we can initialize it with a bigger size, e.g., 997)
  - Hash function: key % 7
  - E.g., after add 26, try position 5, then 5+1, (5+4)%7, the load factor will be over 0.5, we need to resize
 - <img width="677" alt="Screen Shot 2022-07-14 at 4 38 57 PM" src="https://user-images.githubusercontent.com/89602311/179090689-4d0441f3-05f2-423d-b855-4efdb9de5bba.png">
## Double hashing
* Double hashing is performed as follows:
  - If there is a collision at hash address h for the key k, a second hash function can be appliedf to probe location h + ihash2(k)
    - The increment function is ihash2(k)
      - h + hash2(k), h + 2hash2(k), h+ + 3hash2(k), etc.
    - A good choiceL hash2(k) = R - (k%R), where R is a prime number smaller than the hash size, such that hash2(k) will never be 0, and this probes a new location
* Example: lets do not resize, just to illustrate double hashing
  - Keys: 14, 19, 5, 26, 63, 8
  - Hash table size: 7
  - Hash function: key % 7
  - Second hash function: 5 - (key%5)
    - to add 63, try 63%7=0, 0+5-(6355)=2, 0+2*(5-(63%5))=4
  - <img width="604" alt="Screen Shot 2022-07-14 at 4 44 18 PM" src="https://user-images.githubusercontent.com/89602311/179091380-47dcc794-2a65-42d0-b686-e4d57780ff4d.png">
## Deletion
* Problem in the example of linear probing for deletion 
  - If 5 is deleted, and we want to find 26
    - The index for 5 is physically emptied, and the linear search stops at an empty index and this we can't find 26
  - <img width="501" alt="Screen Shot 2022-07-14 at 4 46 04 PM" src="https://user-images.githubusercontent.com/89602311/179091590-4ab9d4e5-2bde-47e3-80be-50ed04f534ac.png">
* Solution
  - Lazy deletion: just mark the index as deleted, but don't physically removing it 
    - This can be applied to all open addressing methods
  - <img width="496" alt="Screen Shot 2022-07-14 at 4 46 50 PM" src="https://user-images.githubusercontent.com/89602311/179091697-8bb9ae60-f47d-4a1c-987c-ae9a6ecab15c.png">
    - A means active, E means empty, and D means deleted
  - for chaining-based hashing, deletion is the same as deletion on a linked list
## Analysis of Hashing
