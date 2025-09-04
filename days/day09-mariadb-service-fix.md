# Day 09 — MariaDB Service Fix

## Question
There is a critical issue going on with the Nautilus application in Stratos DC.  
The production support team identified that the application is unable to connect to the database.  
After digging into the issue, the team found that `mariadb` service is down on the database server.  

**Task:** Look into the issue and fix the same.

---

## Environment
- Host: `stdb01.stratos.xfusioncorp.com`
- Service: `mariadb.service`
- User: `peter` (sudo → root)

---

## My Answer (steps & commands)

```bash
# 1. Checked service status
systemctl status mariadb
journalctl -xeu mariadb.service
tail -n 200 /var/log/mariadb/mariadb.log

# Found errors:
# - Permission denied creating /run/mariadb/mariadb.pid
# - Missing Aria logs → "Unknown storage engine 'Aria'"

# 2. Fixed PID/socket location in config
vi /etc/my.cnf.d/mariadb-server.cnf
# Changed to:
# socket=/var/run/mysqld/mysqld.sock
# pid-file=/var/run/mysqld/mysqld.pid
# log-error=/var/log/mariadb/mariadb.log

# 3. Recreated runtime directory
install -d -m 0755 -o mysql -g mysql /var/run/mysqld

# 4. Cleared stale PID/socket + corrupted Aria logs
rm -f /var/run/mysqld/mysqld.pid /var/lib/mysql/mysql.sock
rm -f /var/lib/mysql/aria_log.* /var/lib/mysql/aria_log_control /var/lib/mysql/tc.log
chown -R mysql:mysql /var/lib/mysql

# 5. Reloaded systemd and restarted service
systemctl daemon-reload
systemctl restart mariadb
systemctl enable mariadb

# 6. Verified service
systemctl is-active mariadb   # active
ss -lntp | grep 3306          # port listening
mysqladmin --socket=/var/run/mysqld/mysqld.sock ping
# mysqld is alive

# 7. Logged into MariaDB and confirmed version
mysql -u root -S /var/run/mysqld/mysqld.sock
SHOW DATABASES;
SELECT VERSION();
```
## Verification
```bash
systemctl status mariadb      # active (running)  
systemctl is-enabled mariadb  # enabled  
ss -lntp | grep 3306          # listening on 3306  
mysqladmin ping               # mysqld is alive  
```
## Output
- Service is running  
- Port 3306 open  
- MariaDB version 10.5.27  
- Databases visible: `mysql`, `information_schema`, `performance_schema`  

---

## Notes / Learning
- Always check `/var/log/mariadb/mariadb.log` and `journalctl -xeu mariadb.service` for real errors.  
- PID and socket files must be under `/var/run/mysqld/` with `mysql:mysql` ownership.  
- Corrupted/missing **Aria logs** (`aria_log.*`) prevent MariaDB startup; deleting them forces regeneration.  
- After crash recovery, confirm with `mysqladmin ping` and SQL queries. 
