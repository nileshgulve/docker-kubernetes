https://docs.docker.com/engine/install/rhel/
https://github.com/gbhure/docker-demo


****************************************

docker run -d --name myubuntu ubuntu sleep 30
docker rm myubuntu
docker run -d -it --name myubuntu ubuntu
docker stop myubuntu
docker rm myubuntu
docker run -d -it --name myubuntu -p 80:80 ubuntu
 vi index.html
docker cp index.html myubuntu:/var/www/html/

docker commit myubuntu myubuntu-apache2:1.0
docker login
docker push myubuntu-apache2:1.0
docker push gbhure/myubuntu-apache2:1.0
docker tag myubuntu-apache2:1.0 gbhure/myubuntu-apache2:1.0
docker tag myubuntu-apache2:1.0 new-apache:1.1
docker images
docker push gbhure/myubuntu-apache2:1.0
   
docker save myubuntu-apache2:1.0 -o myubuntu-apache2.1.0.tar
docker load -i myubuntu-apache2.1.0.tar

********** Docker Volume ***********

docker run -d --name mysql -e MYSQL_ROOT_PASSWORD=root mysql
docker exec -it mysql bash
docker stop mysql
docker rm mysql
docker run -d --name mysql -v /opt/data/:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=root mysql
docker exec -it mysql bash
  
    mysql -u root -proot
        
mysql> use mysql;
mysql> create table empl_data (id INT, name VARCHAR(20), salary INT);
mysql> insert into empl_data values(1001, 'Aakash', 12000);
mysql> insert into empl_data values(1002, 'Muralidhar', 13300);
mysql> insert into empl_data values(1003, 'John', 10190);
mysql> select * from empl_data;
mysql> quit;


**************************************************************
# Exercise 
#MongoDB
docker pull mongo:latest
#mapping local /backup folder to DB 
docker run -d \
  --name mongodb-demo \
  -p 27017:27017 \
  -v /backup:/data/db \
  mongo:latest

docker exec -it mongodb-demo bash 
#logging to mongo shell
mongosh
use demoDB
db.createCollection("demoCollection")
db.demoCollection.insert({ name: "Demo", type: "XYZ" })
exit


docker stop mongodb-demo
docker rm mongodb-demo

#restoring data of deleted container using /backup folder on local
docker run -d \
  --name mongodb-copy \
  -p 27017:27017 \
  -v /backup:/data/db \
  mongo:latest


docker exec -it mongodb-copy bash 

mongosh
#verify restored data
show databases
use demoDB
show collections
db.demoCollection.find().pretty()

*****************************************


Install docker-compose
sudo curl -L "https://github.com/docker/compose/releases/download/v2.23.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/bin/docker-compose
chmod +x /usr/bin/docker-compose
docker-compose --version

******************* Docker Swarm ***************

# docker swarm init --advertise-addr IP-address
Swarm initialized: current node (txte9dgd27n3d5y1ki2hhcvjg) is now a manager.

To add a worker to this swarm, run the following command:

    docker swarm join --token SWMTKN-1-35r19410isww22u7bmctb8rj4jzlkw5tfdjkrst83k09ywiqfd-6h5dcm4lheb4xmyw8yy5lfizg IP-address:2377


# docker service create --name C1 nginx

# docker service ls

# docker service ls

# docker scale C1:5 -- scale up

# docker scale C1:1 -- scale down

# docker service rm C1
