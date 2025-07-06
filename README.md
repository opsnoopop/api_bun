# Bun API with MySQL

A simple Bun API application and MySQL, containerized with Docker.


## Technology Stack

**Bun Container: FROM oven/bun:1**
- OS Debian GNU/Linux: 12
- Bun: 1.2.16
- MySQL2: 3.14.1

**MySQL Container: FROM mysql:8.4.5**
- OS Oracle Linux Server: 9.6
- MySQL: 8.4.5

**grafana/k6 Container: FROM grafana/k6:1.1.0**
- OS Alpine Linux: 3.22.0
- grafana/k6: 1.1.0


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


## Test Performance by grafana/k6

### grafana/k6 test Health Check
```bash
docker run \
--name container_k6 \
--rm \
-it \
--network global_bun \
-v ./k6/:/k6/ \
grafana/k6:1.1.0 \
run /k6/k6_bun_health_check.js
```

### grafana/k6 test Insert Create user
```bash
docker run \
--name container_k6 \
--rm \
-it \
--network global_bun \
-v ./k6/:/k6/ \
grafana/k6:1.1.0 \
run /k6/k6_bun_create_user.js
```

### grafana/k6 test Select Get user by id
```bash
docker run \
--name container_k6 \
--rm \
-it \
--network global_bun \
-v ./k6/:/k6/ \
grafana/k6:1.1.0 \
run /k6/k6_bun_get_user_by_id.js
```

### check entrypoint grafana/k6
```bash
docker run \
--name container_k6 \
--rm \
-it \
--entrypoint \
/bin/sh grafana/k6:1.1.0
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