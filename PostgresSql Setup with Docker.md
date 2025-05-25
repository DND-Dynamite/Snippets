To run PostgreSQL in Docker, follow these steps:

---

### **1. Pull the PostgreSQL Docker Image**

```sh
docker pull postgres:latest
```

This fetches the latest PostgreSQL image.

---

### **2. Run a PostgreSQL Container**

```sh
docker run -d \
  --name postgres \
  -e POSTGRES_USER=postgres \
  -e POSTGRES_PASSWORD=postgres \
  -e POSTGRES_DB=postgres \
  -p 5432:5432 \
  -v "D:\Workspace\postgresql\data" \
  postgres:latest
```

``` sh
>docker run -d --name postgres -e POSTGRES_USER=postgres -e POSTGRES_PASSWORD=postgres -e POSTGRES_DB=postgres -p 5432:5432  -v "D:\Workspace\postgresql\data" postgres:latest
```
#### **Explanation:**

- `-d` → Run in detached mode (background).
- `--name my_postgres` → Name of the container.
- `-e POSTGRES_USER=myuser` → Sets the PostgreSQL user.
- `-e POSTGRES_PASSWORD=mypassword` → Sets the PostgreSQL password.
- `-e POSTGRES_DB=mydatabase` → Creates a database.
- `-p 5432:5432` → Maps the container port 5432 to host 5432.
- `-v pgdata:/var/lib/postgresql/data` → Persists data using a Docker volume.

---

### **3. Verify Running Container**

```sh
docker ps
```

---

### **4. Connect to PostgreSQL**

#### **Using Docker CLI**

```sh
docker exec -it my_postgres psql -U myuser -d mydatabase
```

#### **Using psql on Host Machine**

```sh
psql -h localhost -p 5432 -U myuser -d mydatabase
```

---

### **5. Stop & Remove Container**

```sh
docker stop my_postgres
docker rm my_postgres
```

---

### **6. Persistent Storage**

If you restart the container, your data persists using the volume `pgdata`. If you want a local directory instead, replace:

```sh
-v pgdata:/var/lib/postgresql/data
```

with:

```sh
-v /path/to/local/dir:/var/lib/postgresql/data
```

---

Let me know if you need further assistance! 🚀


To pull the official PostgreSQL image from Docker Hub, use the following command:

```sh
docker pull postgres:latest
```

### **Alternative Versions**

If you need a specific version, specify it like this:

```sh
docker pull postgres:16
```

For a list of available versions, visit:  
🔗 [PostgreSQL on Docker Hub](https://hub.docker.com/_/postgres)

Once pulled, you can verify the image using:

```sh
docker images | grep postgres
```

Would you like help running the container after pulling it? 🚀