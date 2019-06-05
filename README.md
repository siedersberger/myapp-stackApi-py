# My crawilling with python and mongodb (mycrawling-py)

## Description
This project use the stackoverflow API for get informations about the questions made on a specific period of time.
It simulated an environment where there are an application connected with a database and running on *127.0.0.1:5000*. 
Also, there is a bot that store data in database independently of the application.

For this, the project is composed by the follow modules:
- **Database**: MongoDB;
- **Crawl**: responsible for collect and store data from stackoverflow API;
- **App**: responsible for the interface with the user (the app runs on *127.0.0.1:5000*).

Each module runs into a docker container, wich is necessary for run the app.

## App start
To start the application, you have to execute the follow steps:
1. Clone or download this repository
2. Into the directory, execute these commands:
    1. docker-compose build *(docker-compose version: '3.0')*
    2. docker-compose up -d
    3. docker-compose down
3. Go to web browser on **127.0.0.1:5000**

### Operation
While the App module show informations stored in database , the Crawl module stores the data independently. When you run the project with docker commands cited above, the Crawl module runs with default inputs (MONTH and YEAR), storing initial data in the database.  
If you want to add more data to the database, you have to change these inputs parameters in docker-compose.yaml and up the Crawl module again. 

1. Go to the docker-compose.yaml file
2. Set the environment variables MONTH and YEAR for the pycrawl service

```yaml
pycrawl:
    build:
      dockerfile: crawl.Dockerfile
      context: .
    image: daniel/pycrawl
    container_name: pycrawl
    volumes:
      - "./crawl:/var/www/crawl"
    networks:
      - production-network
    depends_on:
      - mongodb
    environment: 
      - MONTH=2
      - YEAR=2019
 ```
3. execute on the terminal **docker-compose up pycrawl** 
