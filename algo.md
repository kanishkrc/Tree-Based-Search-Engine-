# KD-Tree & RP-Tree Searching Algorithm

## KD-Tree / RP-Tree Search Process
1. **Descent Phase**  
   - Start at the root and descend the tree to locate the leaf node where the `testVector` would fall.  
   - Examine points in this leaf to find an initial set of **k nearest neighbors (k-NN)**.  
   - Define the **initial search radius** as the distance to the farthest neighbor (`bestDistance`).  

2. **Backtracking Phase**  
   - Move back up the tree from the leaf.  
   - At each visited parent node:  
     - Check the points stored in that node.  
     - Update the k-NN list if any closer points are found.  

3. **Split Distance Check (Pruning Rule)**  
   - For each node, compute **splitDistance** = distance from `testVector` to that node’s splitting boundary.  
   - **Case 1: `splitDistance < bestDistance`**  
     - The other (unexplored) branch **may** contain closer neighbors.  
     - Descend into and search that branch.  
   - **Case 2: `splitDistance ≥ bestDistance`**  
     - The other branch cannot contain closer points.  
     - **Prune** (ignore) that branch.  

This pruning step is the key optimization.

---

## Split Rule (ChooseRule) Logic

### KD-Tree ChooseRule (Deterministic, Axis-Aligned)
1. For each dimension:  
   - Compute **spread** = (max value – min value).  
2. Select the dimension with the **largest spread** → `splitDim`.  
3. Find the **median value** of the points along `splitDim`.  
4. Split rule:  
   - `"vector[splitDim] ≤ median ? left : right"`  

👉 Splits are axis-aligned, chosen intelligently based on data spread.

---

### RP-Tree ChooseRule (Randomized, Arbitrary-Angled)
1. Generate a random **direction vector v** (same dimension as data).  
2. For each point:  
   - Compute the **dot product** with vector `v`.  
3. Find the **median** of these projected values.  
4. Split rule:  
   - `"dot(vector, v) ≤ median ? left : right"`  

👉 Splits are along random directions, not aligned to axes.

---

✅ **Summary**  
- **KD-Tree** → Deterministic, axis-aligned, based on largest spread.  
- **RP-Tree** → Randomized, arbitrary-angled, based on projections.  
