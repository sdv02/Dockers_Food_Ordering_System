# Dockers_food_ordering_system


Dockerizing DBMS project.

Create a new folder  called 'project'. 
Create a folder called 'src' inside the 'project' folder and add the contents of your project inside the src file.

Open the project folder in VSCode editor.
Create a new file inside project folder called ‘Dockerfile’ and add the below lines in the file.This file is created to containerize the project.

FROM php:7.4-apache
RUN docker-php-ext-install mysqli

Create another file called ‘docker-compose.yml’  and add the below lines. This file helps in containerizing the project and accessing various services. Here we have used 3 services , php for the php files, db for mysql files and adminer for the phpMyAdmin. The code can be found in the dockerhub website , by searching for the service required.(for eg. mysql and the code for services will be found in the website)

version: "3.1"

services:
  php:
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - 80:80
    volumes:
      - ./src:/var/www/html/

  db:
    image: mysql
    # NOTE: use of "mysql_native_password" is not recommended: https://dev.mysql.com/doc/refman/8.0/en/upgrading-from-previous-series.html#upgrade-caching-sha2-password
    # (this is just an example, not intended to be a production configuration)
    command: --default-authentication-plugin=mysql_native_password
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: example

  adminer:
    image: adminer
    restart: always
    ports:
      - 8080:8080

Save the project and run the below commands.
If there are any pre existing containers and if u need to stop them and remove those containers, u can use the following commands.
>docker stop <Container ID>
>docker rm <Container ID>

To run the commands open new terminal in VSCode and give the following commands.
$ docker-compose up -d

Once, the above command is successfully completed , u can use the below command to get information about the containers created.
$ docker ps

Do the following changes to the index.php file and instead of localhost in server name use container name and port number as shown below , and change the password to ‘example’.

$server_name="<container name>:port";
$username="root";
$password="example";
$database_name="<database name>";
$conn=new mysqli($server_name,$username,$password,$database_name);

The project is uploaded to dockers. The containers are displayed in the docker desktop (or docker hub).
The Website can be accessed at localhost:80 and the database can be accessed at localhost:8080.
Create the database(or import the database and tables) and table in localhost:8080 to function the website.

To stop the containers the below command can be used.
$ docker-compose down
