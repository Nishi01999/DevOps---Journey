
# 🧩 **Task 1: Create Compose File**
👉 Create file:
```bash
nano docker-compose.dev.yml
```
👉 Paste:
version: '3.8'

services:
  db:
    image: mysql:8
    container_name: dev-mysql
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: myapp
    volumes:
      - devdata:/var/lib/mysql
    networks:
      - dev-network

  adminer:
    image: adminer
    container_name: dev-adminer
    ports:
      - "8080:8080"
    depends_on:
      - db
    networks:
      - dev-network

volumes:
  devdata:

networks:
  dev-network:
```

---
# ▶️ **Task 2: Start Application**

```bash
docker compose -f docker-compose.dev.yml up -d
```
---
# 🔍 **Task 3: Verify Containers**

```bash
docker compose -f docker-compose.dev.yml ps
```

👉 Both should be **running**

---

# 🌐 **Task 4: Access Adminer**

👉 Open browser:

```
http://localhost:8080
```

---

# 🔐 **Task 5: Login**

Enter:

* System: MySQL
* Server: **db**
* Username: root
* Password: root
* Database: myapp
---
# 🧪 **Task 6: Create Data**

👉 Inside Adminer:

1. Create table → `students`
2. Add columns:

   * id (INT)
   * name (VARCHAR)
3. Insert 2 records
4. Run SELECT
<img width="1260" height="751" alt="image" src="https://github.com/user-attachments/assets/45b0cbfa-4936-4721-8c63-d1a1a87525ff" />

---

# 🔄 **Task 7: Restart Test**

```bash
docker compose -f docker-compose.dev.yml down
docker compose -f docker-compose.dev.yml up -d
```

👉 Check:
* Data still exists? ✅
---
# ❌ **Task 8: Break & Fix (Important)**

👉 Edit file:

Change:

```yaml
MYSQL_DATABASE: wrongdb
```

Restart:

```bash
docker compose -f docker-compose.dev.yml down
docker compose -f docker-compose.dev.yml up -d
```
👉 Try login → FAIL ❌

👉 Fix it back → SUCCESS ✅

# 🎯 **Final Outcome**

You should be able to:

✔ Run multi-container apps
✔ Access DB via browser
✔ Understand Compose networking
✔ Debug basic issues

