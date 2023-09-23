# Data Structures: No thread safety or concurrency
1. List Interface:both of not sync and mutable. linklist performing better when insertion at the end or start, elements maintain order of insertion
	- ArrayList: A resizable array, which can grow as needed.
		- dynamic resizing, when growing or shrinking
		- random access(indexed access) O(1)
		- maintain the order of insertion
		- addall removeall
		- get and set constant time, add or remove o(n) in worst case
	- LinkedList: A doubly-linked list that can also be used as a stack, queue, or double-ended queue.
		- add, get, set, addFirst, addLast, pollFirst, pollLast()
		- each element is a node
		- dynamic resizing, when growing or shrinking
		- indexed access support but O(n)
		- adding and removing generally constant time O(1)
2. Set Interface:
	- HashSet:
		- No dublication allowed most known feature
		- order not guarenteed
		- can store only one null element
		- hashset backedup by a hashmap. keys are elements and values are constant value.
		- not sync and not thread safe.
		- add remove contains supports
	- LinkedHashSet: Similar to HashSet, 
		- it maintains the order of insertion.
		- it also uses linkedlist to connect hashed object to each others
	- TreeSet: A sorted set backed by a red-black tree.
3. Queue Interface:
	- LinkedList: As mentioned above, can be used as a queue.
	- PriorityQueue: 
		- ordered element by priority or by a comparator, its maintain largest or smallest easyly
		- not thread safe
		- not null allowance
		- inserting or removing o(LogN)
4. Dequeue Interface:
	- LinkedList: Can be used as a double-ended queue.
	- ArrayDequeue: A resizable-array implementation of a double-ended queue.
		- as a stack push(e) pop peek
		- as a queue offer(e) poll peek
5. Map Interface:
	- HashMap: A hash table based map.
		- key-value storage
		- no duplicate keys**
		- allowance null key once and multiple null values.
		- it does not maintain order
		- constant time to get and put.
	- LinkedHashMap: 
		- Preserves the insertion order.by using linkedlist
		- put and get contanst time also linked list insertion  additionally
	- TreeMap: A map backed by a red-black tree.
6. Specialized Data Structures:
	- EnumSet: A Set for use with enum types.
	- EnumMap: A Map optimized for enum keys.
	- BitSet: A vector of bits that grows as needed.
# Concurrent Data Structures
1. ## Concurrent Queues:
    - ConcurrentLinkedQueue: A non-blocking concurrent queue using linked nodes.
    - LinkedBlockingQueue: A classic blocking bounded FIFO (First-In-First-Out) queue.
    - ArrayBlockingQueue: A bounded blocking FIFO queue backed by an array.
    - PriorityBlockingQueue: An unbounded blocking priority queue.
2. ## Concurrent Deques:
    - ConcurrentLinkedDeque: A non-blocking concurrent deque using linked nodes.
    - LinkedBlockingDeque: A blocking deque backed by linked nodes.
3. ## Concurrent Maps:
    - ConcurrentHashMap: A high-performance concurrent hash map.
    - ConcurrentSkipListMap: A concurrent version of a SkipListMap. This data structure provides expected averag