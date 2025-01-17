Each `InnoDB` table has a special index called the clustered index that stores row data. Typically, the clustered index is synonymous with the primary key.

When you define a `PRIMARY KEY` on a table, `InnoDB` uses it as the clustered index.

Indexes other than the clustered index are known as secondary indexes.

If the primary key is long, the secondary indexes use more space, so it is advantageous to have a short primary key.

 You can have only one clustered index per table because you can’t store the rows in two places at once.