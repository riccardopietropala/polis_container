# polis_container

Install docker and docker swarm and docker-compose to run container on 


Polis Container using docker-compose
 
  copy docker-compose.yml
  
    docker-compose up  
    
    or 
    
    docker-compose up -d
    
Create docker-compose.yml using following -
----------------------------------------
    
        version: '2'
        services:
            # Polis PostgreSQL Database
            polisdb:
                image: uzzal2k5/polis_postgresql:1.0
                container_name: polisdb
                ports:
                  - "5432:5432/tcp"
                restart: on-failure

            # Polis Server
            polisserver:
                image: uzzal2k5/polis_server:1.0
                container_name: polisserver
                ports:
                  - "5000:5000/tcp"
                links:
                  - polisdb
                environment:
                  DATABASE_URL: postgres://polis:polis@polisdb/polis
                  STATIC_FILES_HOST: clientparticipation
                restart: on-failure

            # Polis Client Admin & Participants
            clientparticipation:
                image: uzzal2k5/polis_adminparticipation:1.0
                container_name: clientparticipation
                hostname: clientparticipation
                environment:
                  SERVICE_URL: polisserver
                links:
                  - polisserver
                ports:
                  - "5001:5001"
                  - "5002:5002"
                  - "8000:8000"
                restart: on-failure

            # Polis Math Container
            math:
                image: uzzal2k5/polis_math:1.0
                container_name: math
                hostname: math
                environment:
                  SERVICE_URL: polisserver
                  DATABASE_URL: postgres://polis:polis@polisdb/polis
                links:
                  - polisserver
                  - polisdb
                ports:
                  - "8080:8080"
                restart: on-failure




Build you own Image
-----------------------

Build polis_postgresql

      
    git clone https://bitbucket.org/shafiq2k5/polispostgres.git
        cd   polispostgres
        docker build -t polis_postgresql .
        
Build polis_leiningen 
   
    git clone https://bitbucket.org/shafiq2k5/polisleiningen.git
    cd  polisleiningen
    docker build -t polis_leiningen .
 
Build polis_adminparticipants   
    
    git clone https://bitbucket.org/shafiq2k5/polis_adminparticipants.git
    cd  polis_adminparticipants
    docker build  -t polis_adminparticipants .

Build polis_server  
    
    git clone https://bitbucket.org/shafiq2k5/polisserver.git
    cd polisserver
    docker build -t polis_server .
Build polis_math
 
    git clone https://bitbucket.org/shafiq2k5/polismath.git
    cd  polismath
    docker build polis_math .
     
    