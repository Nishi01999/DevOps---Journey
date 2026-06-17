Step 1: Create Volume
docker volume create mysql-data
Step 2: Run MySQL Container
docker run -d \
--name mysql-db \
-e MYSQL_ROOT_PASSWORD=root123 \
-v mysql-data:/var/lib/mysql \
-p 3306:3306 \
mysql:8.0
Step 3: Verify
docker ps
Step 4: Enter Container
docker exec -it mysql-db bash
Step 5: Login to MySQL
mysql -u root -p
Step 6: Create Database
CREATE DATABASE school;
Step 7: Stop Container
docker stop mysql-db
Step 8: Start Container Again
docker start mysql-db
Step 9: Verify Data Still Exists
SHOW DATABASES;

