
### What is MySQL replication and how does it work?

MySQL replication is a feature that allows you to maintain copies of your database on multiple servers, ensuring data consistency and high availability. It works by designating one server as the source and others as replicas. The source server records changes in its binary log, and replicas copy these changes and apply them to their own data. This asynchronous process allows replicas to be updated without impacting the source's performance, though some lag may occur.

### What are the advantages of using MySQL replication?

MySQL replication offers several advantages, including:

- **High Availability:** Replicas can act as failover servers, minimizing downtime during maintenance or unexpected outages.
- **Read Scaling:** Replicas can handle read queries, offloading the source server and improving performance for read-intensive applications.
- **Data Distribution:** Replicas can be located geographically distant from the source, facilitating data localization and disaster recovery.
- **Backup and Analytics:** Replicas can serve as dedicated servers for backups and analytical queries, isolating these tasks from the primary database.

### What are the different replication formats available in MySQL?

MySQL supports three binary log formats for replication:

- **Statement-Based Replication (SBR):** Records the actual SQL statements executed on the source. It's compact but can have issues with non-deterministic queries.
- **Row-Based Replication (RBR):** Records changes at the row level, ensuring deterministic replication but potentially increasing binary log size.
- **Mixed Replication:** Attempts to combine SBR and RBR, using SBR by default and switching to RBR when necessary. This can lead to unpredictable behavior and is generally not recommended.

### What are Global Transaction Identifiers (GTIDs) and why are they important?

GTIDs are unique identifiers assigned to every transaction committed on the source server. They simplify replication management by eliminating the need to track binary log filenames and positions. Replicas can easily identify and apply transactions based on their GTIDs, even after source server restarts or binary log rotations.

### How can I make MySQL replication more resilient to crashes?

To improve replication crash safety:

- Set innodb_flush_log_at_trx_commit = 1 and sync_binlog = 1 to ensure data durability and minimize data loss in case of crashes.
- Use relay_log_info_repository = TABLE and relay_log_recovery = ON for atomic updates and automatic relay log recovery after crashes.

### What is delayed replication and when is it useful?

Delayed replication introduces a time delay between the source and replica. It's beneficial for mitigating data loss scenarios, such as accidental data deletion. By retrieving data from the delayed replica up to a point before the error, you can quickly recover lost data. However, delayed replication adds complexity to administration and monitoring.

### How can I monitor replication lag and ensure data consistency between replicas and the source?

- **Monitoring Lag:** Implement a heartbeat mechanism using tools like pt-heartbeat to accurately measure replication lag. Monitor disk space usage for binary logs and relay logs.
- **Data Consistency:** Enforce super_read_only on replicas to prevent accidental writes. Use row-based replication or deterministic statements to ensure consistent data updates. Avoid writing to multiple servers within a replication topology.

### What are some common replication problems and how can I troubleshoot them?

- **Corrupted Binary Logs:** Requires rebuilding replicas from a backup.
- **Non-unique Server IDs:** Verify and assign unique server IDs to each replica.
- **Missing Temporary Tables:** Use row-based replication or exclude temporary tables from replication.
- **Excessive Replication Lag:** Implement multithreaded replication, consider sharding, or temporarily lower durability settings on replicas (with caution).
- **Oversized Packets:** Ensure consistent max_allowed_packet values between source and replicas.
- **Disk Space Issues:** Monitor disk usage, set appropriate relay_log_space limits, and manage binary log purging.