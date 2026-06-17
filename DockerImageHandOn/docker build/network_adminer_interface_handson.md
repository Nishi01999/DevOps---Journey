

🟢 Step 1: Create DB Container
docker run -d \
  --name mysql-db \
  -e MYSQL_ROOT_PASSWORD=root \
  -e MYSQL_DATABASE=myapp \
  -p 3307:3306 \
  mysql

🟢 Step 2: Insert Data (CLI Only)
docker exec -it mysql-db mysql -uroot -proot 
USE myapp;
CREATE TABLE users (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(50)
);
INSERT INTO users (name) VALUES ('Ravi'), ('Sita');
SELECT * FROM users;
"

“Data is now stored inside the container”




🔵 Step 3: Create Network
create a network so containers can talk
docker network create myapp-net

🔵 Step 4: Run Web UI (Adminer)
docker run -d \
  --name adminer \
  --network myapp-net \
  -p 8082:8080 \
  adminer

🔵 Step 5: Connect DB to Network
docker network connect myapp-net mysql-db

🌍 Step 6: Browser Demo
👉 Open:
http://localhost:8082
Login:
Server: mysql-db
Username: root
Password: root
Database: myapp
👉 Show users table


“Browser → Adminer → MySQL → Data → Back to browser”

🔴 Step 7: Break It (Delete DB)

“delete the container”
docker rm -f mysql-db


💾 Step 8: Fix with Volume

“make data permanent using volumes”
docker volume create mydata

docker run -d \
  --name mysql-db \
  --network myapp-net \
  -v mydata:/var/lib/mysql \
  -e MYSQL_ROOT_PASSWORD=root \
  -e MYSQL_DATABASE=myapp \
  mysql

🟢 Step 9: Insert Data Again (CLI)
docker exec -it mysql-db mysql -uroot -proot -e "
USE myapp;
CREATE TABLE users (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(50)
);
INSERT INTO users (name) VALUES ('Ravi'), ('Sita');
"

🔁 Step 10: Delete & Recreate (With Volume)
docker rm -f mysql-db

docker run -d \
  --name mysql-db \
  --network myapp-net \
  -v mydata:/var/lib/mysql \
  -e MYSQL_ROOT_PASSWORD=root \
  -e MYSQL_DATABASE=myapp \
  mysql

🎉 Step 11: Verify Data (CLI)
docker exec -it mysql-db mysql -uroot -proot 
USE myapp;
SELECT * FROM users;
"
👉 Data still exists ✅

