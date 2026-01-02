# Preorder, Inorder, and Postorder Traversals in One Traversal

**Goal:** To generate Preorder, Inorder, and Postorder traversals of a Binary Tree using a **single stack** and a **single pass** (loop), rather than running three separate algorithms.

## 1. Core Concept & Data Structure
To achieve all three traversals simultaneously, we simulate the recursion stack explicitly. We track the "state" of our visit to each node to determine which traversal list (Preorder, Inorder, or Postorder) needs to be updated.

*   **Data Structure:** A **Stack** that stores a `Pair`.
    *   **Format:** `Pair<Node, Number>` (or `Pair<Node, State>`),.
    *   **Initialization:** Push the Root node with an initial state (number) of **1**.
*   **Output Structures:** Three separate lists/vectors to store the Preorder, Inorder, and Postorder sequences.

## 2. The Three States
The algorithm checks the `Number` (state) associated with the `Node` at the top of the stack. Depending on whether the number is 1, 2, or 3, specific actions are taken.

### State 1: Preorder (Root → Left → Right)
This state represents the **first visit** to the node.
*   **Action:** Add the node's value to the **Preorder** list.
*   **Update Stack:** Increment the node's state from **1 to 2**.
*   **Movement:** If the node has a **Left** child, push the Left child onto the stack with an initial state of **1**,.

### State 2: Inorder (Left → Root → Right)
This state represents returning to the node after visiting its left subtree.
*   **Action:** Add the node's value to the **Inorder** list,.
*   **Update Stack:** Increment the node's state from **2 to 3**.
*   **Movement:** If the node has a **Right** child, push the Right child onto the stack with an initial state of **1**,.

### State 3: Postorder (Left → Right → Root)
This state represents returning to the node after visiting both left and right subtrees.
*   **Action:** Add the node's value to the **Postorder** list,.
*   **Update Stack:** **Pop** the node from the stack completely. No state increment is needed as we are finished with this node,.
*   **Movement:** Do not push any new children.

## 3. Algorithm Execution Flow (Dry Run Logic)
The process runs inside a loop until the stack is empty.

1.  **Start:** Push `(Root, 1)` to the stack.
2.  **Loop:** Get the top element `(CurrentNode, State)`.
    *   **If State == 1:**
        *   Store `CurrentNode` in Preorder list.
        *   Update `State` to 2.
        *   Push `(CurrentNode.Left, 1)` if it exists.
    *   **If State == 2:**
        *   Store `CurrentNode` in Inorder list.
        *   Update `State` to 3.
        *   Push `(CurrentNode.Right, 1)` if it exists.
    *   **If State == 3:**
        *   Store `CurrentNode` in Postorder list.
        *   Pop the element from the stack.

## 4. Complexity Analysis

### Time Complexity: $O(3N) \approx O(N)$
*   We visit every node in the binary tree.
*   For every node, we essentially perform operations corresponding to state 1, state 2, and state 3.
*   While strictly $3N$ iterations occur, in Big-O notation, this simplifies to **$O(N)$**.

### Space Complexity: $O(N)$
*   We use a stack to store nodes. In the worst-case scenario (a skewed tree), the stack depth will be proportional to the number of nodes.
*   We also require space for the three output lists (Preorder, Inorder, Postorder).

## 5. Summary Table

| State Number | Traversal Type | Action with Value | Next Step for Node | Next Movement |
| :--- | :--- | :--- | :--- | :--- |
| **1** | **Preorder** | Add to List | Update State to 2 | Push **Left** Child (init 1) |
| **2** | **Inorder** | Add to List | Update State to 3 | Push **Right** Child (init 1) |
| **3** | **Postorder** | Add to List | **Pop** from Stack | None |
