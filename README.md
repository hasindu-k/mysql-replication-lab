# 🐬 MySQL Replication Lab with Docker

This project sets up a **basic MySQL master-replica (primary-secondary) replication** environment using **Docker Compose**. It's perfect for learning how database replication works in a hands-on way.

---

## 🧠 What You'll Learn

- How MySQL replication works (binary log-based)
- How to configure a master and replica server
- How to test data replication locally
- How to use Docker Compose to manage DB environments

---

## 📦 Project Structure

```
mysql-replication-lab/
├── docker-compose.yml
├── master-init/
│   └── 1-init.sql
└── slave-init/
    └── 1-init.sql
```

---

## 🚀 Quick Start

### 1. Clone the Repo

```bash
git clone https://github.com/your-username/mysql-replication-lab.git
cd mysql-replication-lab
```

### 2. Start the MySQL Containers

```bash
docker compose up -d
```

This will start:

- `mysql-master` on port `3307`
- `mysql-slave` on port `3308`

### 3. Configure Replication

#### Get Master Log Info

```bash
docker exec -it mysql-master mysql -uroot -prootpass
SHOW BINARY LOG STATUS;

OR

docker exec -it mysql-master mysql -uroot
SHOW BINARY LOG STATUS;
```

Note the `File` and `Position` values (e.g., `mysql-bin.000003` and `158`).

#### Set Up the Slave

```bash
docker exec -it mysql-slave mysql -uroot -prootpass

CHANGE REPLICATION SOURCE TO
  SOURCE_HOST='mysql-master',
  SOURCE_USER='repl',
  SOURCE_PASSWORD='replpass',
  SOURCE_LOG_FILE='mysql-bin.000003',
  SOURCE_LOG_POS=158;

START REPLICA;
```

Verify with:

```sql
SHOW REPLICA STATUS\G
```

---

## ✅ Test Replication

Insert a row in the **master**:

```sql
USE testdb;
CREATE TABLE demo (id INT PRIMARY KEY, name VARCHAR(50));
INSERT INTO demo VALUES (1, 'Hello Replication!');
```

Now check the **replica**:

```sql
USE testdb;
SELECT * FROM demo;
```

You should see the same data appear on the replica server ✨

---

## 🧹 Clean Up

To stop and remove the containers:

```bash
docker compose down -v
```

---

## 📚 Resources

- [MySQL Replication Docs](https://dev.mysql.com/doc/refman/8.0/en/replication.html)
- [Docker Compose](https://docs.docker.com/compose/)
- [MySQL on Docker Hub](https://hub.docker.com/_/mysql)

---

## 📄 License

MIT – Use, modify, and learn freely.
