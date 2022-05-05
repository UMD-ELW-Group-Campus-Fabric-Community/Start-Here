
## Remote connect

ssh -i docker-students.pem ubuntu@18.234.199.79
ssh -i docker-students.pem ubuntu@3.215.148.52

## 
git clone https://github.com/UMD-ELW-Group-Campus-Fabric-Community/Database-1.git

mkdir envfiles && cd $_

touch db.dev.env
---
POSTGRES_USER=admin
POSTGRES_PASSWORD=INST490@Group16
POSTGRES_DB=wcfc_db
---

touch server.dev.env
---
DB_HOST=coop_db
DB_PORT=5432
DB_USER=admin
DB_PASS=INST490@Group16
DB_NAME=wcfc_db
---

sudo lsof -i :80
sudo kill <pid>
sudo yarn build
sudo screen yarn start

--- dont forget to clean up old images! 
sudo docker sytsem prune
