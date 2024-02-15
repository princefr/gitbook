# Skip List

The **skip list** can support fast insertion, deletion, and search operations. Skip lists are used in the implementation of sorted sets in Redis.

**Description**: Searching in a singly linked list has a time complexity of O(n). In a skip list, every two nodes in the singly linked list are extracted to form an upper level, which is called the index level. Nodes in the index level have a `down` pointer pointing to the next level's node, a `next` pointer pointing to the next node in the current index level, and a value representing the sorted value. On the first index level, every two nodes form a node in the higher level, creating a second index level. This process continues, adding multiple index levels.

**Time complexity of search**: Assuming there are n data points, there are `n / 2` nodes in the first index level, and `n / (2 ^ k)` nodes in the kth index level. Assuming there are h levels, with 2 nodes in the highest level, `h = log2(n) - 1`. Adding the original level, the height of the skip list is log2(n). **At most 3 nodes are traversed on each level**, resulting in a time complexity of `O(3 * log2(n)) = O(logn)`.

**Space complexity**: `n + n / 2 + n / 4 + n / 8 + ... = n - 2`, which is O(n). In practice, space usage is not a major concern because the nodes in the original linked list are usually large objects, while the nodes in the index levels only need to store key values and pointers, without the need to store the entire object.

**Time complexity of insertion**: The time complexity of simply inserting into a singly linked list is O(1), but finding the insertion position takes O(n), resulting in a total complexity of O(n). In a skip list, finding the position takes O(logn), and the insertion itself takes O(1), resulting in a **total complexity of O(logn)**.

**Deletion operation**: In addition to deleting nodes in the original linked list, nodes in the index levels need to be deleted if they exist.

**Dynamic updates**: When continuously inserting nodes, it is possible to have a large number of data points between two index nodes, which may reduce query efficiency. Skip lists maintain balance through a random function. For example, when inserting a node, a random function generates K, and then this node is added to the index levels from the first level to the Kth level. The selection of the random function is crucial, as it ensures the balance between the size of the skip list's index and the size of the data, preventing excessive performance degradation.

**Why Redis sorted sets choose skip lists over red-black trees**:

1. The Redis sorted set API includes operations such as insertion, deletion, search, search by range, and iterative output of ordered sequences. Except for search by range, other operations can also be accomplished with red-black trees, and the time complexity is the same. However, **skip lists are more efficient for search by range**.
2. The **implementation of skip lists is simpler**.

{% hint style="info" %}
Skip lists cannot completely replace red-black trees because red-black trees have been around for a long time, and many programming languages implement maps using red-black trees, which can be used directly. Skip lists need to be implemented manually.
{% endhint %}