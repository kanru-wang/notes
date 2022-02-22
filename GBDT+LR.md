From: https://blog.csdn.net/xxiaobaib/article/details/100049720
- When training data is very imbalanced and has many dimensions, signal-by-chance can very often happen. Now the normalization mechanics in Logistic Regression (LR) is more useful than that in Gradient Boosting Decision Tree (GBDT), because (in my understanding) normalization forces LR to spread weights on all by-chance and not-by-chance signals, but there is no normalization that can prevent GBDT from splitting at a perfect by-chance signal at a shallow node. My guess is that Random Forest which is free from this problem, but is less accurate than GBDT + LR.

From: https://towardsdatascience.com/next-better-player-gbdt-lr-for-binary-classification-f8dc6f32628e
- There is a good code example. The key is to use `pred_leaf=True` which is to use to output the index of prediction leaf node for each tree.
- (1) get leaf node index, (2) one-hot encoding the list of leaf node index (3) feed the final list to LR
- `# of leaf nodes` is not `# of prediction classes`
- `# of leaf trees`  =  `length of the list of leaf node index`
- `# of leaf nodes`  x  `# of trees`  =  `length of final list`

From: https://zhuanlan.zhihu.com/p/57987311
- Interesting review on some implementation techniques mentioned in the Facebook CTR GBDT + LR paper