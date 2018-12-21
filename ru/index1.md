# What is ClickHouse?

ClickHouse is a column-oriented database management system (DBMS) for online analytical processing of queries (OLAP).

In a "normal" row-oriented DBMS, data is stored in this order:

| Row | WatchID     | JavaEnable | Title              | GoodEvent | EventTime           |
| --- | ----------- | ---------- | ------------------ | --------- | ------------------- |
| #0  | 89354350662 | 1          | Investor Relations | 1         | 2016-05-18 05:19:20 |
| #1  | 90329509958 | 0          | Contact us         | 1         | 2016-05-18 08:10:20 |
| #2  | 89953706054 | 1          | Mission            | 1         | 2016-05-18 07:38:00 |
| #N  | ...         | ...        | ...                | ...       | ...                 |

In order words, all the values related to a row are physically stored next to each other.

Examples of a row-oriented DBMS are MySQL, Postgres, and MS SQL Server. {: .grey }

In a column-oriented DBMS, data is stored like this:

| Row:        | #0                  | #1                  | #2                  | #N  |
| ----------- | ------------------- | ------------------- | ------------------- | --- |
| WatchID:    | 89354350662         | 90329509958         | 89953706054         | ... |
| JavaEnable: | 1                   | 0                   | 1                   | ... |
| Title:      | Investor Relations  | Contact us          | Mission             | ... |
| GoodEvent:  | 1                   | 1                   | 1                   | ... |
| EventTime:  | 2016-05-18 05:19:20 | 2016-05-18 08:10:20 | 2016-05-18 07:38:00 | ... |

These examples only show the order that data is arranged in. The values from different columns are stored separately, and data from the same column is stored together.

Examples of a column-oriented DBMS: Vertica, Paraccel (Actian Matrix and Amazon Redshift), Sybase IQ, Exasol, Infobright, InfiniDB, MonetDB (VectorWise and Actian Vector), LucidDB, SAP HANA, Google Dremel, Google PowerDrill, Druid, and kdb+. {: .grey }

## Key Properties of the OLAP scenario

- The vast majority of requests are for read access.
- Data is updated in fairly large batches (> 1000 rows), not by single rows; or it is not updated at all.
- Data is added to the DB but is not modified.

![Row-oriented](./images/row_oriented.gif#)

Example

<details>

<p>
  <pre>$ clickhouse-client
ClickHouse client version 0.0.52053.
Connecting to localhost:9000.
Connected to ClickHouse server version 0.0.52053.

:) SELECT CounterID, count() FROM hits GROUP BY CounterID ORDER BY count() DESC LIMIT 20

SELECT
CounterID,
count()
FROM hits
GROUP BY CounterID
ORDER BY count() DESC
LIMIT 20

┌─CounterID─┬──count()─┐
│    114208 │ 56057344 │
│    115080 │ 51619590 │
│      3228 │ 44658301 │
│     38230 │ 42045932 │
│    145263 │ 42042158 │
│     91244 │ 38297270 │
│    154139 │ 26647572 │
│    150748 │ 24112755 │
│    242232 │ 21302571 │
│    338158 │ 13507087 │
│     62180 │ 12229491 │
│     82264 │ 12187441 │
│    232261 │ 12148031 │
│    146272 │ 11438516 │
│    168777 │ 11403636 │
│   4120072 │ 11227824 │
│  10938808 │ 10519739 │
│     74088 │  9047015 │
│    115079 │  8837972 │
│    337234 │  8205961 │
└───────────┴──────────┘

20 rows in set. Elapsed: 0.153 sec. Processed 1.00 billion rows, 4.00 GB (6.53 billion rows/s., 26.10 GB/s.)

:)</pre>
</p>

</details>

### CPU

    #some_code_block
    

[cross_link](./operations/table_engines/merge_tree.md#merge_tree) [cross_link_2](../a/b.md#b) [external_link](https://wiki.org)