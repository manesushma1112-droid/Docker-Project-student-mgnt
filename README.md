Docker Multi-Tier Application Deployment
1. Create Docker Network
docker network create student-app-network

Verify:
docker network ls


2. Build PostgreSQL Image
docker build -t my-postgres-image ./database

Verify:docker build -t my-postgres-image ./database
docker images


3. Create Volume
docker volume create pgdata

Verify:
docker volume ls


4. Run PostgreSQL Container
docker run -d \
  --name postgres-db \
  --network student-app-network \
  -p 5432:5432 \
  -v pgdata:/var/lib/postgresql/data \
  my-postgres-image

Verify:
docker ps

Check Logs:
docker logs -f postgres-db


5. Build Backend Image
docker build -t flask-backend ./backend

Verify:
docker images


6. Run Backend Container
docker run -d \
  --name flask-api \
  --network student-app-network \
  -p 5000:5000 \
  -e DATABASE_URL=postgresql://admin:123456@postgres-db:5432/mydatabase \
  flask-backend

Verify:
docker ps

Check Logs:
docker logs -f flask-api

Test API:
curl http://localhost:5000

curl http://localhost:5000/students


7. Build Frontend Image
docker build -t student-frontend ./frontend

Verify:
docker images


8. Run Frontend Container
docker run -d \
  --name frontend-ui \
  --network student-app-network \
  -p 8080:80 \
  student-frontend

Verify:
docker ps

Check Logs:
docker logs -f frontend-ui


9. Access Application
Frontend:
http://localhost:8080

Backend API:
http://localhost:5000

Students API:
http://localhost:5000/students


10. Verify Container Communication
Inspect Network:
docker network inspect student-app-network

Expected Containers:
postgres-db
flask-api
frontend-ui


11. Verify Database
Connect to PostgreSQL:
docker exec -it postgres-db psql -U admin -d mydatabase

Show Tables:
\dt

Check Students:
SELECT * FROM students;

Check Items:
SELECT * FROM items;


12. Cleanup Commands
Stop Containers:
docker stop frontend-ui flask-api postgres-db

Remove Containers:
docker rm frontend-ui flask-api postgres-db

Remove Images:
docker image rm student-frontend flask-backend my-postgres-image

Remove Volume:
docker volume rm pgdata

Remove Network:
docker network rm student-app-network

