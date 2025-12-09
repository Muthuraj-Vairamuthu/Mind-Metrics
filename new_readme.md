| Purpose            | Technique       |
| ------------------ | --------------- |
| Scale data size    | Random Sampling |
| Reduce dimensions  | JL Transform    |
| Matrix compression | Randomized SVD  |



Why XGBoost Was Faster Than Random Forest?

XGBoost uses gradient boosting with histogram-based and approximate split finding, reducing per-tree computation. Random Forest trains trees independently using bagging, which increases memory and computation overhead. This explains why XGBoost achieved higher test accuracy with significantly lower training time.

# Asymptotic Running Time (Big-O) of XGBoost

The **approximate training time complexity** of XGBoost is:

**O(n ⋅ d ⋅ log(n) ⋅ T)**

### Parameters
- **n** = number of training samples (≈ 202,944 in your case)  
- **d** = number of features (21)  
- **T** = number of boosting trees (typically 100–1000+)

### Where does each term come from?
- `n ⋅ d` → evaluating each feature for every sample at each split candidate
- `log(n)` → originates from histogram-based (approximate) split finding and sorting/partitioning of data (similar to depth of a balanced tree)
- `T` → total number of trees built sequentially in gradient boosting

This makes XGBoost **near-linear** in practice with respect to the number of samples, especially when using the approximate (histogram) algorithm (default for large datasets).

### Practical Implications
XGBoost scales very well to hundreds of thousands or even millions of rows, unlike many traditional algorithms.

### Complexity Comparison

| Algorithm          | Training Time Complexity              | Feasibility for ~200k–250k rows |
|--------------------|----------------------------------------|----------------------------------|
| **XGBoost**        | O(n ⋅ d ⋅ log(n) ⋅ T)                  | Very good (minutes to hours)    |
| **Random Forest**  | O(n ⋅ d ⋅ T ⋅ log(n)) + bagging overhead | Good, but slower than XGBoost  |
| **RBF Kernel SVM** | O(n²) to O(n³)                         | Infeasible (days to impossible) |
| **Linear SVM**     | O(n ⋅ d) (with linear kernel)          | Fast, but often lower performance |

### Summary
Thanks to its optimized exact/approximate greedy split finding, second-order gradients, and built-in regularization, **XGBoost is significantly faster** than traditional implementations of gradient boosting (GBM) and most other non-linear models while often delivering superior predictive performance.