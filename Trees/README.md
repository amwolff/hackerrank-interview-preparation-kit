# Trees

## [Tree: Height of a Binary Tree](https://www.hackerrank.com/challenges/tree-height-of-a-binary-tree/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=trees)

```cpp
int height(Node* root) {
  int left = 0, right = 0;
  if (root->left != NULL) {
    left = 1 + height(root->left);
  }
  if (root->right != NULL) {
    right = 1 + height(root->right);
  }
  return std::max(left, right);
}
```

## [Binary Search Tree : Lowest Common Ancestor](https://www.hackerrank.com/challenges/binary-search-tree-lowest-common-ancestor/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=trees)

```cpp
Node *lca(Node *root, int v1, int v2) {
  if (v1 > root->data && v2 > root->data && root->right != NULL) {
    return lca(root->right, v1, v2);
  } else if (v1 < root->data && v2 < root->data && root->left != NULL) {
    return lca(root->left, v1, v2);
  }
  return root;
}
```

## [Trees: Is This a Binary Search Tree?](https://www.hackerrank.com/challenges/ctci-is-binary-search-tree/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=trees)

```cpp
bool checkNodeBounds(Node* root, int min, int max) {
  if (root->data < min || root->data > max) {
    return false;
  }
  bool left = true, right = true;
  if (root->left != NULL) {
    left = checkNodeBounds(root->left, min, root->data - 1);
  }
  if (root->right != NULL) {
    right = checkNodeBounds(root->right, root->data + 1, max);
  }
  return left && right;
}

bool checkBST(Node* root) { return checkNodeBounds(root, 0, 10000); }
```

## [Tree: Huffman Decoding](https://www.hackerrank.com/challenges/tree-huffman-decoding/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=trees)

```cpp
void decode_huff(node *root, string s) {
  string decodedStr;
  node *temp = root;
  for (auto c : s) {
    if (c == '0') {
      temp = temp->left;
    } else {
      temp = temp->right;
    }
    if (temp->left == NULL && temp->right == NULL) {
      decodedStr += temp->data;
      temp = root;
    }
  }
  std::cout << decodedStr << std::endl;
}
```
