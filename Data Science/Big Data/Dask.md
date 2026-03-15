# Architecture
High-level APIs
	`dask.array`: Parallel NumPy
	`dask.bag`: Parrallel lists
	`dask.dataframe`: Parallel Pandas
	Dask ML: Parallel scikit-learn
Low-level APIs
	Dask Delayed
	Dask Futures
Dask Subsystem
	Scheduler
# Grammar
```python
import dask.dataframe as dd
ddf = dd.from_prandas(df, npartitions=10)
```