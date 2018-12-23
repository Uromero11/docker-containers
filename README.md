# Docker Containers 
Web dev Containers

### Requisites
- Docker
- Docker Compose

### Technology
- PHP 7.2
- MySQL 5.6

### Setup
1 - Create entries in the /etc/hosts file
```bash
127.0.0.1 nuevo-projecto.local
```

2 - Clone this repository in some directory in your local environment

3 - Using the command line go to tge directory where this project was cloned
```bash
# build the containers (the first time)
docker-compose up --build

# after the first time
docker-compose up -d
```


4 - Check if all containers are runing
```bash
docker ps

CONTAINER ID        IMAGE                COMMAND                  CREATED             STATUS              PORTS                                                                    NAMES
```

5 - Clone back end code

```bash
# Change to Projects/ directory 
cd docker-containers/Projects/
```


6 - Clone nuevo-projecto code
```bash
# Change to Projects/ directory 
cd Projects/ 
```



### Nginx container
#### Check logs
```bash
# SSH to nginx container
docker exec -it *_nginx_1 bash

# Tail the access and error logs
tail -f log/log/nuevo-projecto/*.log
```

### PHP Container 
```bash
# SSH to php fpm 
docker exec -it *_phpfpm_1 bash

# code
nuevo-projecto/
```

### Data base container
```bash
docker exec -t *_mysql_1 bash

# access db inside container
mysql -uroot -p <db name>
```
#### Access database outside the container 
```bash
mysql -uroot -P3307 -h127.0.0.1 -p local_pricing
```

### Test in browser
```
http://nuevo-projecto.local:8081
```