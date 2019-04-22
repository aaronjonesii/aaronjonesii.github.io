---
layout: post
title: Reset MySQL Auto Increment
image: https://user-images.githubusercontent.com/18272237/52320193-f3e43600-299b-11e9-81d4-5007fe237ec2.png
---

While developing my WebApp, I ran into multiple posts being in my database that I created for testing purposes only.
This poses a problem when I am ready for production use, to delete all records and reset the the auto increment follow the steps below:

```bash
# Login to MySQL Server
mysql -u root -p
```
Now execute the statements below:
```sql
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

The table is now empty and ready for production.
