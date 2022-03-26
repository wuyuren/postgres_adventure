# Steps to setup postgres and import adventure works. ,
1. Inspect the docker-compose.yaml file in this folder. If you want to change password for database root user and/or pg_admin web page admin user, feel free to do so . 
2. Use the docker compose command to start the postgres database container and pg_admin container 
 command: 

    ```docker-compose up```
3. Use browser to login to local host : http://localhost:5050   Check the docker-compose.yaml file for the pg_admin username and password settings at line 18 and 19
4. Use the pg admin gui to add a server 
    
    4.1 server ip address : use docker inspect command to identify the IPAddress value in the output json 
    ```docker inspect pg_container```

    4.2 database username and password: check the docker-compose.xml file line 8 and 9

5. Copy the adventure setup script and csv data to the database container
    5.1 create a temporary folder in container 
    ```
    docker exec -it pg_container bash -c "mkdir /tmp/adventure"
    ```
    5.2 copy file 
    ```
    docker cp ./psotgres-data/* pg_container:/tmp/adventure
    ```
6. Execute pgsql to create database and run setup scripts 
    6.1 create database 
    ```
     docker exec -it pg_container psql postgres -c "CREATE DATABASE adventureworks;"
    ```
    6.2 run setup script
    ```
    docker exec -it  pg_container  bash -c "cd /tmp/adventure && psql -d adventureworks < install.sql"
    ```
# Postgres adventure database setup script and data file is from https://github.com/lorint/AdventureWorks-for-Postgres

