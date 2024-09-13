# Row Oriented Databases
- Tables are stored as rows in disk.
- A single block io read to the table fetches multiples rows with all their columns.
- More IOs are required to find a particular row in a table scan but once the find the row you get all columns for that row.


# Column Oriented Databases
- Tables are stored as columns first in disk.
- A single block io read to the table fetches multiple columns with all matching rows.
Less IOs are required to get more values of a given column. But working with multiple columns require more IOs.
OLAP.

## Pros and Cons
### Row Based
- Optimal for read/writes
- OLTP
- Compression isn't efficient.
- Aggregation isn't efficient.
- Efficient queries with multi columns.

### Column Based
- Column Based
- Writes are slower.
- OLAP
- Compress greatly
- Amazing for aggregation.
- Inefficient queries with multiple columns.

