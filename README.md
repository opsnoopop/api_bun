# Bun API with MySQL

A simple Bun API application and MySQL, containerized with Docker.

## Technology Stack

**Bun Container: FROM oven/bun:1**
- Bun: 1.2.16
- MySQL2: 3.14.1
- Debian GNU/Linux 12 (bookworm)

**MySQL Container: FROM mysql:8.4.5**
- MySQL: 8.4.5


## Getting Started

### 1. Clone the Repository
```bash
git clone https://github.com/opsnoopop/api_bun.git
```

### 2. Navigate to Project Directory
```bash
cd api_bun
```

### 3. Start the Application
```bash
docker compose up -d --build
```

### 4. Create table users
```bash
docker exec -i container_mysql mysql -u'root' -p'password' testdb -e "
CREATE TABLE testdb.users (
  user_id INT NOT NULL AUTO_INCREMENT ,
  username VARCHAR(50) NOT NULL ,
  email VARCHAR(100) NOT NULL ,
  created_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP ,
  PRIMARY KEY (user_id)
) ENGINE = InnoDB;
"
```


## API Endpoints

### Health Check
- **URL:** http://localhost:3001/
- **Method:** GET
- **Response:**
```json
{
  "message": "Hello World from Bun"
}
```

### Create user
- **URL:** http://localhost:3001/users
- **Method:** POST
- **Request**
```json
{
  "username":"optest",
  "email":"auttakorn.w@clicknext.com"
}
```
- **Response:**
```json
{
  "message":"User created successfully",
  "user_id":1
}
```

### Get user
- **URL:** http://localhost:3001/users/1
- **Method:** GET
- **Response:**
```json
{
  "user_id":1,
  "username":"optest",
  "email":"auttakorn.w@clicknext.com"
}
```


## Stop the Application

### Truncate table users
```bash
docker exec -i container_mysql mysql -u'root' -p'password' testdb -e "
Truncate testdb.users;
"
```

### Delete table users
```bash
docker exec -i container_mysql mysql -u'root' -p'password' testdb -e "
DELETE FROM testdb.users;
"
```

### Stop the Application
```bash
docker compose down
```