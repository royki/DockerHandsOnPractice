#### Run to create MySql container from docker hub
- `docker run --name mysql-basic \
    -e MYSQL_USER=user1 -e MYSQL_PASSWORD=mypa55 \
    -e MYSQL_DATABASE=items -e MYSQL_ROOT_PASSWORD=r00tpa55 \
    -d mysql:5.6`

- `docker run --name mysql-basic -e MYSQL_USER=user1 -e MYSQL_PASSWORD=mypa55 -e MYSQL_DATABASE=items -e MYSQL_ROOT_PASSWORD=r00tpa55 -d mysql:5.6`

- Access the container sandbox by running the following command:
	- `docker exec -it mysql-basic bash`

- Log in to MySQL Container as the database administrator user (root).Run the following command from the container terminal to connect to the database:
	- `mysql -pr00tpa55`
- Create a new table in the items database. From the MySQL prompt, run the following command to access the database:
	- `use items;`
- From the MySQL prompt, create a table called Projects in the items database:
	- `CREATE TABLE Courses (id int NOT NULL, name varchar(255) NOT NULL, PRIMARY KEY (id));`
- Run the following command to verify that the table was created:
	- `show tables;`
- Insert a row in the table by running the following command:
	- `insert into Courses (id, name) values (1, 'DO081x');`
- Run the following command to verify that the project information was added to the table:
	- `select * from Courses;`