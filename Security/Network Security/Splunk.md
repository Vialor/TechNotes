# SPL Search Processing Language
`index=botsv1 AND source=... AND sourceType=... | stats count by src_ip | ...`
|: pipe
stats: command
count: function
by: clause
## Basic transforming commands and searches
Transforming commands are used to order the results of a search into statistical data tables. As the name implies, these commands “transform” specified event data cell values into numerical values/data structures which Splunk can interpret and use for statistical purposes. This is required for charts and other kinds of data visualizations. Below are some of the most common transforming commands:
#### Chart
The `chart` command can display any series of data you wish to plot in the form of a chart. The command is designed to work with statistical functions such as `min()`, `count()`, `sum(`). The `over` keyword is used to determine what field takes the x-axis. The following search displays unique instances of client IP averaged over each weekday (unique visitors to a site).
```bash
index=dataset | chart avg(clientip) over date_wday 
```
#### Timechart
The `timechart` command is used to create reports on certain trends in your data over time. The command is designed to work with statistical functions such as `min()`, `count()`, `sum()`. The x-axis of any chart created with the `timechart` command will always be the default field `_time`.
```bash
index=dataset | timechart span=12h count BY status
```
#### Stats
The `stats` command is used to create a report that displays summary statistics about your data. The command is designed to work with statistical functions such as `min()`, `count()`, `sum()`.
```bash
index=dataset | stats count BY status#Or for a specific field/value.index=dataset status=404 | stats count as “Not Found”
```
#### Top
The `top` command generates charts that show the most common values of a specified field. By default, the command returns the top 10 values. This behavior can be altered with the `limit` constraint.
```sql
index=dataset | top dst_port limit=5
```
#### Rare
The `rare` command is used to generate charts that display the least common values of a specified field. The options available to the command are identical to those for the `top` command above.
```sql
index=dataset | rare host showperc=f limit=1
```