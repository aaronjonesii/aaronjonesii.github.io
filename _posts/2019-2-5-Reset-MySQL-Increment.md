---
layout: post
title: Reset MySQL Auto Increment
---

While developing my WebApp, I ran into multiple posts being in my databse that I created for testing purposes only.
This poses a problem when I am ready for production use, to delete all records and reset the the auto increment follow the steps below:

```bash
# Login to MySQL Server
mysql -u root -p
```
```SQL
# Select correct Database
USE <Database name>;

# Shows tables of selected Database
SHOW TABLES;

# Count records in Table of Database
SELECT COUNT(*) FROM <Table name>;

# Delete all records from table
DELETE FROM <Table name>;

# Reset MySQL Auto Increment
ALTER TABLE api_post AUTO_INCREMENT = 1;
```

Now the table is empty and ready for production.
