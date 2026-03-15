Assumptions: all models in the ensemble have no correlation with each other
## Instance Bagging
> Bootstrap based on samples

**Bootstrapping**: Given one dataset, create multiple datasets by sampling data randomly with replacement
**Parallel training**: Train base learners for each datasets independently
**Aggregation**: The result is the average result of learners (regression), or the majority result of the learners (classification)
## Feature Bagging
> Bootstrap based on features

Decision Tree --> **Random Forest**: for each training tree, pick a random set of features at each split
	Usually, choose $\sqrt d$  features from a total of d features