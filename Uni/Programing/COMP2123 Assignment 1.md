**SID : 550686527**

## Problem 1
**a) Tight Bound Time Complexity** 
The tight bound time complexity for $FOO(u)$, where $u$ is the root, is $\Theta (n\log{n})$.

---

**b) Explanation**
For a given node $u$, we are told $BAR(u)$ runs in $\Theta (|T_u|)$, where $T_u$ is the subtree rooted at node $u$, and $|T_u|$ is the size of the subtree rooted at node $u$. If $u$ is the root, than the function $BAR(u)$ runs $n$ times, where $n$ is the number of nodes in the tree. So, $FOO(u)$ has a time complexity of $n \times |T_u|$, where we can consider the size of this expression. If we switch to thinking about the amount of times a node is counted to make up the work of $n \times |T_u|$, we find each node is counted based on its appearance in each subtree - which is equivalent to the number of its ancestors - or its depth in the tree plus the one instance it is called for its own subtree. Therefore each node $u$ is essentially counted $depth(u)+1$ times (considering root as $depth=0$). For a binary tree, the height of a node in a tree of size $n$ is at most $\log{n}$. Therefore the considering the upper bound of the depth of nodes in a BT we generalise to $\log{n}$, and to search for a node in a BT takes $O(\log{n})$ time. Counting a node $u$ its $depth(u)$ times is equivalent to traversing down a tree to that node, which we've discussed takes $O(\log{n})$, so we for each node working its depth+1. Mathematically, this is  written as:
$$\sum_{u}|T_u| = \sum_{\text{nodes}}O(\text{depth}) = O(n\log{n})$$
To prove this algorithm is tight, we must prove it cannot be faster. Consider a balanced BT. Since this tree has height $\Theta(\log n)$, then at least half the nodes have depth $\Omega(\log n)$ (property of balanced trees), where each such node contributes $\Omega(\log n)$ of work, thus the total work is  $\Omega(n\log n)$. 
Therefore:  $$\Omega(n\log n) = O(n\log{n}) = \Theta(n\log{n})$$



## Problem 2

**(a) Algorithm**
We are given two binary search trees $T_1$ and $T_2$ on disjoint keys. We determine whether they can be connected using a single edge such that the resulting tree satisfies the binary search tree property. The key observation is that each position in a binary search tree corresponds to an interval of allowable values. A subtree can be attached at a position if and only if all its keys lie strictly within the interval defined by that position. We first compute the minimum and maximum keys of $T_2$, denoted $m = \min(T_2)$ and $M = \max(T_2)$. We then traverse $T_1$, maintaining for each node an interval $(\ell, r)$ representing the allowable range of values for any subtree rooted at that node. Initially, at the root, the interval is $(-\infty, +\infty)$.
At a node with key $k$, we check:
- whether $T_2$ can be attached as a left child, i.e. $\ell < m \leq M < k$,
- whether $T_2$ can be attached as a right child, i.e. $k < m \leq M < r$.
If either condition holds and the corresponding child is empty, we conclude that the trees are compatible. Otherwise, we recurse into the corresponding subtree, updating the interval to $(\ell, k)$ when moving left and $(k, r)$ when moving right, but only if the interval condition could still be satisfied. If no valid position is found in $T_1$, we repeat the same procedure with the roles of $T_1$ and $T_2$ reversed. The trees are compatible if and only if a valid attachment point is found in either direction.

---

**(b) Correctness**
We prove correctness by showing both directions.
Suppose the algorithm returns that the trees are compatible. Then it has found a node with allowable interval $(\ell, r)$ such that the range of keys in the other tree, say $[m, M]$, satisfies
$$ℓ<m≤M<r$$
If the tree is attached as a left child of a node with key $k$, then $M < k$, so all keys in the attached tree are strictly less than $k$, and $m > \ell$, so they also satisfy all constraints imposed by ancestors. The argument is symmetric for attachment as a right child. Therefore, the binary search tree property is preserved at the attachment point and throughout the tree, and the resulting structure is a valid binary search tree. Conversely, suppose that the trees are compatible. Then there exists a node $v$ in one of the trees such that attaching the other tree at $v$ yields a valid binary search tree. Let $(\ell, r)$ be the allowable interval at $v$. Since the resulting tree satisfies the binary search tree property, all keys of the attached tree must lie within this interval, so
$$ℓ<min⁡(T2)≤max⁡(T2)<r$$
During execution, the algorithm explores exactly those nodes whose intervals could contain the range $[m, M]$, and therefore it will reach node $v$. At this node, the interval condition holds, and the algorithm identifies a valid attachment point. Hence, if a valid attachment exists, the algorithm will find it.

Therefore, the algorithm is correct.

---

**(c) Time Complexity**
The worst-case running time occurs when $T_1$ and $T_2$ are not compatible, so the algorithm must attempt to attach each tree to the other. To determine whether one tree can be attached to the other, we first compute the minimum and maximum of a binary search tree, which requires traversing a single root-to-leaf path. This takes time proportional to the height of the tree.
Thus, computing all required extrema gives:
$$\text{min } T_1 + \text{max } T_2 + \text{min } T_2 + \text{max } T_1 $$$$= O(\text{height} T_1) + O(\text{height} T_2) + O(\text{height} T_2) + O(\text{height} T_1)$$$$= O(2\text{height} T_1 + 2\text{height} T_2)$$
The algorithm then traverses each tree along a single path determined by interval constraints, which also takes time proportional to the height of the tree. Considering the worst case where the algorithm runs on both trees, the total running time is:
$$O(2\text{height} T_1 + 2\text{height} T_2) + O(\text{height} T_1) + O(\text{height} T_2)$$
BST trees averaged height follow the form $\text{height}(T) = O(\log n)$, giving:


$$= O(2\log{(n_{T1}}) +2\log{(n_{T2}})) + O(\log{(n_{T1}})) + O(\log{(n_{T2}}))$$
$$= O(3\log{(n_{T1}}) + 3\log{(n_{T2}}))$$
Generalise:
$$= O(\log{(n_{T1}}) + \log{(n_{T2}}))$$
Hence, the overall running time is:
$$O(\text{height} T_1) + O(\text{height} T_2)$$
