---
layout: post
comments: true
categories: quiz
---
## Invert Tree

```

  // Definition for a binary tree node.
//      4
//    /   \
//   2     7
//  / \   / \
// 1   3 6   9
//      4
//    /   \
//   7     2
//  / \   / \
// 9   6 3   1
class TreeNode {
    left: TreeNode | null
    right: TreeNode | null
    val: number
    constructor(val?: number, left?: TreeNode | null, right?: TreeNode | null) {
        this.val = (val===undefined ? 0 : val)
        this.left = (left===undefined ? null : left)
        this.right = (right===undefined ? null : right)
    }
}
 
function invertTree(root: TreeNode | null): TreeNode | null {
  if(root === null) {return null}
  return new TreeNode(root.val, invertTree(root.right), invertTree(root.left))
};
const aTree = new TreeNode(4, new TreeNode(2, new TreeNode(1), new TreeNode(3)), new TreeNode(7, new TreeNode(6), new TreeNode(9)))
console.log(invertTree(aTree))
```
