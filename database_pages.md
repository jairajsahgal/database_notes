# Database Pages — A Deep Dive

Databases use fixed-size pages to store data. These pages are a fundamental unit for storing and managing data efficiently.

## Page Concept

- **Definition**: Databases read and write data in pages. Everything eventually becomes bytes in a page.
- **Purpose**: Separates the storage engine from the database frontend, which handles data format and API. Facilitates efficient data access and caching.

## Page Operations

1. **Reading Data**
   - Database locates the page where the data resides.
   - Identifies the file and offset on disk.
   - OS reads from the file and brings the page into memory.
   - Page is placed in the buffer pool, allowing efficient access to other rows within the page.

2. **Writing Data**
   - Database finds the page, pulls it into memory.
   - Updates the row in memory.
   - Logs the change in the Write-Ahead Log (WAL) for persistence.
   - Page remains in memory for further changes before being flushed to disk.

## Page Content

- **Row-Store Databases**: Store rows and attributes sequentially in pages. Beneficial for OLTP workloads.
- **Column-Store Databases**: Store data column by column. Enhances OLAP performance by making aggregate functions efficient.
- **Document-Based Databases**: Compress and store documents in pages, similar to row-stores.
- **Graph-Based Databases**: Store graph connectivity in pages, optimized for traversing graphs.

## Page Size Considerations

- **Small Pages**: Faster to read/write, less metadata overhead relative to data.
- **Large Pages**: Reduce metadata overhead and page splits, but increase cold read/write costs.

## Page Size Defaults

- PostgreSQL: 8KB
- MySQL InnoDB: 16KB
- MongoDB WiredTiger: 32KB
- SQL Server: 8KB
- Oracle: 8KB

Adjustments may be necessary based on use case.

## Storage on Disk

- **File Per Table**: Uses fixed-size pages stored in sequential files.
  - **Example**: To read pages 2 through 9 with an 8KB page size, read from offset `2 * 8192` for `8 * 8192` bytes.

## PostgreSQL Page Layout

PostgreSQL default page size is 8KB. The layout includes:

1. **Page Header (24 bytes)**: Metadata including free space information.
2. **ItemIds (4 bytes each)**: Array of pointers to items (offset and length). Supports HOT (Heap Only Tuple) optimization by pointing to new tuples without updating indexes.
3. **Items (Variable Length)**: Contains the actual data.
4. **Special (Variable Length)**: For B+Tree index leaf pages, stores page pointers for links to previous and next pages.

### Criticisms

- **ItemId Size**: 4 bytes per item pointer can be inefficient if many items are stored, leading to higher metadata overhead relative to data.

---

This outline provides a comprehensive overview of how pages are used in databases, their management, and specific details about PostgreSQL's page layout.
