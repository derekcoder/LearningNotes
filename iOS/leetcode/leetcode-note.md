# LeetCode 

## 461 Hamming Distance

The [Hamming distance](https://en.wikipedia.org/wiki/Hamming_distance) between two integers is the number of positions at which the corresponding bits are different. Give two integers `x` and `y`, calculate the Hamming distance.

> Note:
> 0 <= x, y < 2^31

```
Input: x = 1, y = 4
Output: 2

Explanation:
1  (0 0 0 1)
4  (0 1 0 0)
```

My Solutions:

```swift
func hammingDistance(_ x: Int, _ y: Int) -> Int {
    var result = 0
    var z = x ^ y
    while z > 0 {
        if z & 1 == 1 {
            result += 1
        }
        z = z >> 1
    }
    return result
}
```

### 思路

1. 使用位运算符 `异或`，来得到一个新的整数 `z`：该整数对应位置上是 `1`或 `0`，取决于原始两个输入在该位置上是否相同。如果不同，则为`1`，相同为`0`。
2. 让新得到的整数 `z` 与整数 `1` 进行位运算 `与`，来检测`z`的最后一位是否是 `1`。如果是 `1`，结果 `+1`。
3. 整数 `z` 右移一位，继续步骤2。

## 617 Merge Two Binary Trees

Given two binary trees and imagine that when you put one of them to cover the other, some nodes of the two trees are overlapped while the others are not. 

You need to merge them into a new binary tree. The merge rule is that if two nodes overlap, then sum node values up as the new value of the merged node. Otherwise, the NOT null node will be used as the node of new tree.

```
Input: 
	Tree 1                     Tree 2                  
          1                         2                             
         / \                       / \                            
        3   2                     1   3                        
       /                           \   \                      
      5                             4   7                  
Output: 
Merged tree:
	     3
	    / \
	   4   5
	  / \   \ 
	 5   4   7
```

My Solutions:

```swift
public class TreeNode {
    public var val: Int
    public var left: TreeNode?
    public var right: TreeNode?
    
    public init(_ val: Int) {
        self.val = val
        self.left = nil
        self.right = nil
    }
}

public func mergeTrees(_ t1: TreeNode?, _ t2: TreeNode?) -> TreeNode? {
    guard let t1 = t1 else { return t2 }
    guard let t2 = t2 else { return t1 }
    
    let result = TreeNode(t1.val + t2.val)
    
    result.left = mergeTrees(t1.left, t2.left)
    result.right = mergeTrees(t1.right, t2.right)
    
    return result    
}
```

### 思路

使用递归来依次计算对应结点
