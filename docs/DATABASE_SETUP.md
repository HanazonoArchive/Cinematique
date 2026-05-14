# Database Setup Guide

> **📦 ARCHIVED PROJECT:** This guide is provided for reference when studying or running the archived project.

## Overview

Cinematique uses **MySQL** for data persistence. This guide walks through complete database setup.

## Prerequisites

- **MySQL Server** 5.7 or later
- **MySQL Connector/J** (included in lib/)
- **Command-line client** or GUI tool (MySQLWorkbench recommended)

## Installation Steps

### Step 1: Install MySQL

#### Windows
- Download from [mysql.com](https://dev.mysql.com/downloads/mysql/)
- Run installer, choose setup type (Developer Default recommended)
- Configure MySQL Server (port 3306, service)
- Create root account with password

#### macOS
```bash
brew install mysql
brew services start mysql
mysql_secure_installation
```

#### Linux (Ubuntu/Debian)
```bash
sudo apt-get install mysql-server
sudo mysql_secure_installation
```

### Step 2: Start MySQL Service

#### Windows
- Services → MySQL → Start
- Or: `net start MySQL80` (PowerShell Admin)

#### macOS/Linux
```bash
sudo systemctl start mysql
# or
brew services start mysql
```

### Step 3: Connect to MySQL

```bash
mysql -u root -p
# Enter password when prompted
```

## Database Initialization

### Option A: Using SQL Files (Recommended)

**From Command Line:**
```bash
# Connect to MySQL
mysql -u root -p

# Inside MySQL prompt, run:
SOURCE src/main/java/com/schoolproject/database/initDb.sql;
SOURCE src/main/java/com/schoolproject/database/populateDB.sql;
```

**From Files:**
```bash
mysql -u root -p < src/main/java/com/schoolproject/database/initDb.sql
mysql -u root -p < src/main/java/com/schoolproject/database/populateDB.sql
```

### Option B: Using IntelliJ IDEA

1. **Open IntelliJ IDEA**
2. **View → Tool Windows → Database**
3. **Click "+" → Data Source → MySQL**

4. **Configure Connection:**
   - Host: `localhost`
   - Port: `3306`
   - User: `root` (or your username)
   - Password: (your password)
   - Database: Leave blank

5. **Click "Test Connection"** to verify

6. **Open SQL File:**
   - Navigate to `src/main/java/com/schoolproject/database/initDb.sql`
   - Right-click → "Run"
   - Select your MySQL data source
   - Execute

7. **Repeat for populateDB.sql**

### Option C: Using MySQL Workbench

1. **Open MySQL Workbench**
2. **Click your MySQL connection**
3. **File → Open SQL Script**
4. **Select initDb.sql** → Open
5. **Execute (Ctrl+Shift+Enter)**
6. **Repeat for populateDB.sql**

## Database Configuration

### Update Connection Credentials

Edit `src/main/java/com/schoolproject/database/DatabaseConnection.java`:

```java
private static final String JDBC_URL = "jdbc:mysql://localhost:3306/cceprojectdatabase";
private static final String USERNAME = "root";        // Change if needed
private static final String PASSWORD = "your_pass";  // Change if needed
```

⚠️ **Note:** Hardcoded credentials are for development only. Use environment variables for production.

## Database Structure

### Main Tables

#### users
```sql
CREATE TABLE users (
    user_id INT PRIMARY KEY AUTO_INCREMENT,
    username VARCHAR(50) UNIQUE NOT NULL,
    password VARCHAR(100) NOT NULL,
    email VARCHAR(100),
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    role VARCHAR(20), -- 'user' or 'admin'
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

#### movies
```sql
CREATE TABLE movies (
    movie_id INT PRIMARY KEY AUTO_INCREMENT,
    title VARCHAR(255) NOT NULL,
    director VARCHAR(100),
    genre VARCHAR(50),
    release_year INT,
    rating DECIMAL(3,1),
    quantity INT,
    price DECIMAL(10,2),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

#### rentals
```sql
CREATE TABLE rentals (
    rental_id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT,
    movie_id INT,
    rental_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    return_date DATE,
    status VARCHAR(20), -- 'active' or 'returned'
    FOREIGN KEY (user_id) REFERENCES users(user_id),
    FOREIGN KEY (movie_id) REFERENCES movies(movie_id)
);
```

#### reviews
```sql
CREATE TABLE reviews (
    review_id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT,
    movie_id INT,
    rating INT,
    comment TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(user_id),
    FOREIGN KEY (movie_id) REFERENCES movies(movie_id)
);
```

#### logs
```sql
CREATE TABLE logs (
    log_id INT PRIMARY KEY AUTO_INCREMENT,
    action VARCHAR(255),
    user_id INT,
    timestamp TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    details TEXT
);
```

See `initDb.sql` for complete schema.

## Sample Data

### Test Credentials

After running `populateDB.sql`, use these credentials:

**Admin Account:**
```
Username: admin
Password: admin123
```

**Regular User:**
```
Username: john_doe
Password: password123
```

**Note:** These are sample accounts. Change in production.

## Verification

### Verify Tables Created

```sql
USE cceprojectdatabase;
SHOW TABLES;

-- Should output:
-- +---------------------------+
-- | Tables_in_cceprojectdatabase |
-- +---------------------------+
-- | logs                      |
-- | movies                    |
-- | rentals                   |
-- | reviews                   |
-- | users                     |
-- +---------------------------+
```

### Check Sample Data

```sql
SELECT COUNT(*) FROM users;
SELECT COUNT(*) FROM movies;
SELECT COUNT(*) FROM rentals;
```

## Troubleshooting

### Connection Refused
```
Error: java.sql.SQLException: Communications link failure
```
**Solution:**
- Ensure MySQL is running: `mysql --version`
- Start service: `net start MySQL80` (Windows) or `brew services start mysql`
- Check port 3306 is accessible
- Verify credentials in DatabaseConnection.java

### Database Doesn't Exist
```
Error: Unknown database 'cceprojectdatabase'
```
**Solution:**
- Run `initDb.sql` first
- Check SQL file executed without errors
- Verify with: `SHOW DATABASES;`

### Permission Denied
```
Error: java.sql.SQLException: Access denied for user
```
**Solution:**
- Check username and password in DatabaseConnection.java
- Verify MySQL user exists: `SELECT user FROM mysql.user;`
- Grant privileges:
  ```sql
  GRANT ALL PRIVILEGES ON cceprojectdatabase.* TO 'your_user'@'localhost';
  FLUSH PRIVILEGES;
  ```

### Port Already in Use
```
Error: Can't create TCP/IP socket (10048)
```
**Solution:**
- MySQL already running on port 3306
- Or change port in MySQL config and update JDBC_URL
- Check: `netstat -an | findstr :3306` (Windows)

## Resetting Database

To completely reset and start fresh:

```sql
-- Drop database
DROP DATABASE IF EXISTS cceprojectdatabase;

-- Recreate from script
SOURCE src/main/java/com/schoolproject/database/initDb.sql;
SOURCE src/main/java/com/schoolproject/database/populateDB.sql;
```

Or from command line:
```bash
mysql -u root -p -e "DROP DATABASE IF EXISTS cceprojectdatabase;"
mysql -u root -p < src/main/java/com/schoolproject/database/initDb.sql
mysql -u root -p < src/main/java/com/schoolproject/database/populateDB.sql
```

## Backup & Restore

### Create Backup
```bash
mysqldump -u root -p cceprojectdatabase > backup.sql
```

### Restore from Backup
```bash
mysql -u root -p < backup.sql
```

## Additional Resources

- [MySQL Documentation](https://dev.mysql.com/doc/)
- [MySQL Connector/J](https://dev.mysql.com/doc/connector-j/)
- [SQL Tutorial](https://www.w3schools.com/sql/)

---

**For issues or questions, open an issue on GitHub or check README.md**
