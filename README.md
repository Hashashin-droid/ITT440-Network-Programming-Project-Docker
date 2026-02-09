
# ITT440 — Docker Socket Programming Project

## Overview

This project demonstrates client–server communication using **TCP and UDP sockets** implemented in **C and Python**, running inside Docker containers and connected to a shared **MySQL database**.

Each server container periodically updates a user record in the database, while its corresponding client retrieves the latest value.

The system shows how containerized services communicate over a Docker network using hostname resolution.

---

## Architecture

The system consists of:

* **1 MySQL database container**
* **4 server containers**

  * C TCP server
  * C UDP server
  * Python TCP server
  * Python UDP server
* **4 client containers**

  * Each client connects only to its corresponding server

All containers communicate through a custom Docker network.

```
Client → Server → MySQL Database
```

Servers update the database every **30 seconds**, while clients request the latest points.

---

## Features

* TCP & UDP socket programming
* C and Python implementations
* Docker container networking
* MySQL database integration
* Periodic database updates
* Container-to-container communication

---

## Container Mapping

| Language | Protocol | Server Port | Database User |
| -------- | -------- | ----------- | ------------- |
| C        | TCP      | 5001        | ikhwan        |
| C        | UDP      | 5002        | fadhil        |
| Python   | TCP      | 5003        | jerry         |
| Python   | UDP      | 5004        | khalid        |

---

## Requirements

* Docker Engine installed
* Docker CLI access
* Internet connection (for MySQL image)

---

## Setup

### 1️⃣ Create Docker network

```bash
docker network create itt440-net
```

---

### 2️⃣ Run MySQL database container

```bash
docker run -d \
  --name MySql_db \
  --network itt440-net \
  -e MYSQL_ROOT_PASSWORD=root \
  -e MYSQL_DATABASE=itt440 \
  mysql:8
```

---

### 3️⃣ Initialize database

Enter MySQL:

```bash
docker exec -it MySql_db mysql -u root -p
```

Inside MySQL:

```sql
USE itt440;

CREATE TABLE scores (
  user VARCHAR(50),
  points INT,
  datetime_stamp DATETIME
);

INSERT INTO scores VALUES
('ikhwan', 0, NOW()),
('fadhil', 0, NOW()),
('jerry', 0, NOW()),
('khalid', 0, NOW());
```

---

## Running the Demo

### Start servers

```bash
docker start ikhwan-server
docker start fadhil-server
docker start jerry-server
docker start khalid-server
```

---

### Run clients

```bash
docker start ikhwan-client
docker start fadhil-client
docker start jerry-client
docker start khalid-client
```

---

### View client output

```bash
docker logs <client-name>
```

Example:

```
Latest points: 42
```

---

### Verify database updates

Inside MySQL:

```sql
SELECT * FROM scores;
```

Servers update their respective records every 30 seconds.

---

## Project Structure

```
project/
│
├── c-tcp/
│   ├── server.c
│   ├── client.c
│   └── Dockerfile
│
├── c-udp/
│   ├── server.c
│   ├── client.c
│   └── Dockerfile
│
├── python-tcp/
│   ├── server.py
│   ├── client.py
│   └── Dockerfile
│
├── python-udp/
│   ├── server.py
│   ├── client.py
│   └── Dockerfile
│
└── README.md
```

---

## How It Works

### Server behavior

* Listens on assigned port
* Updates database every 30 seconds
* Responds to client requests with latest points

### Client behavior

* Connects to matching server
* Requests latest points
* Displays result

---

## Learning Objectives

This project demonstrates:

* Socket programming fundamentals
* TCP vs UDP communication
* Container networking
* Database integration
* Service orchestration using Docker

---
