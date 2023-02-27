# docker
Demo to create docker-compose to install multiple services

## Installing Gitea with Docker:

ref: https://docs.gitea.io/en-us/install-with-docker/

Gitea runs on http://localhost:3000

    version: "3"

    networks:
      gitea:
        external: false

    services:
      server:
        image: gitea/gitea:1.18.5
      container_name: gitea
      environment:
         - USER_UID=1000
         - USER_GID=1000
      restart: always
      networks:
        - gitea
      volumes:
        - ./gitea:/data
        - /etc/timezone:/etc/timezone:ro
        - /etc/localtime:/etc/localtime:ro
      ports:
        - "3000:3000"
        - "222:22"

## Installing Redmine with Docker:

ref: https://hub.docker.com/_/redmine

Redmine requires database container, so we will use MySQL to run Redmine

Here, I have just changed the port number from "8080" to "10083"

Redmine runs on http://localhost:10083

    version: "3"
    
    services:
      redmine:
        image: redmine
        restart: always
        ports:
          - "10083:3000"
        environment:
          REDMINE_DB_MYSQL: db
          REDMINE_DB_PASSWORD: example
          REDMINE_SECRET_KEY_BASE: supersecretkey

      db:
        image: mysql:5.7
        restart: always
        environment:
          MYSQL_ROOT_PASSWORD: example
          MYSQL_DATABASE: redmine
        
## Installing Sonarqube with Docker:

ref: https://docs.sonarqube.org/9.6/try-out-sonarqube/

Sonarqube runs on: http://localhost:9000

    version: "3"
    
    services:
      sonarqube:
      image: sonarqube:latest
      ports:
        - "9000:9000"
      environment:
        - 'SONAR_ES_BOOTSTRAP_CHECKS_DISABLE=true'
        
## Our docker-compose.yml is:

    version: "3"

    networks:
      gitea:
        external: false

    services:
      server:
        image: gitea/gitea:1.18.5
        container_name: gitea
        environment:
          - USER_UID=1000
          - USER_GID=1000
        restart: always
        networks:
          - gitea
        volumes:
          - ./gitea:/data
          - /etc/timezone:/etc/timezone:ro
          - /etc/localtime:/etc/localtime:ro
        ports:
          - "3000:3000"
          - "222:22"

      redmine:
        image: redmine
        restart: always
        ports:
          - "10083:3000"
        environment:
          REDMINE_DB_MYSQL: db
          REDMINE_DB_PASSWORD: example
          REDMINE_SECRET_KEY_BASE: supersecretkey

      db:
        image: mysql:5.7
        restart: always
        environment:
          MYSQL_ROOT_PASSWORD: example
          MYSQL_DATABASE: redmine

      sonarqube:
        image: sonarqube:latest
        ports:
          - "9000:9000"
        environment:
          - 'SONAR_ES_BOOTSTRAP_CHECKS_DISABLE=true'
          
  To run docker-compose.yml
      
      docker-compose up
      
  ![Screenshot from 2023-02-27 11-39-18](https://user-images.githubusercontent.com/122020679/221489479-16dc179c-7d96-4c37-9135-96e0c28cc6bb.png)

  Containers will take time to run
  
  ![Screenshot from 2023-02-27 11-40-02](https://user-images.githubusercontent.com/122020679/221487856-f8feeccc-a917-4ec0-a4f6-52e3ace6c2bf.png)


  Now, let us see our services:
  
  1. Gitea: http://localhost:3000

  ![Screenshot from 2023-02-27 11-44-54](https://user-images.githubusercontent.com/122020679/221488570-d8ffbcca-1b45-4511-badd-91a918a4f49d.png)

  2. Redmine: http://localhost:10083

     Wait for 2-3 minutes, then refresh the page
    
  3. Sonarqube: http://localhost:9000
  
  ![Screenshot from 2023-02-27 11-47-14](https://user-images.githubusercontent.com/122020679/221488827-cd3aadc5-4e0a-4d84-89ad-7f1a032fc27e.png)
  
  
  Listing containers:
  
    docker ps
    
   ![Screenshot from 2023-02-27 11-49-07](https://user-images.githubusercontent.com/122020679/221489181-29b07794-abf6-4a77-90fb-9ebddd547030.png)

    
    


      
      
