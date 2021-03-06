#Build Podcast ep 043 - SQL
[Screencast link](http://build-podcast.com/sql/)

[SQL](http://en.wikipedia.org/wiki/SQL) is a structured query language for databases. In this episode we will create a simple database on our Solar System planets and their moons. We will explore both a GUI [Sequel Pro](http://www.sequelpro.com/) for viewing SQL as well as [SQLfiddle](http://sqlfiddle.com/), a web based version to run SQL statements. Then we will display the data create using [php](http://www.php.net/manual/en/mysql.examples-basic.php) and [sinatra](http://recipes.sinatrarb.com/p/models/active_record) (ruby).

Version: MySQL 5.5.25

Similar episodes: [009 Package Managers](http://build-podcast.com/package-managers/), [014 Local web server](http://build-podcast.com/local-web-servers/), [040 Sinatra](http://build-podcast.com/sinatra/)

#Background on SQL

1. What is [SQL](http://en.wikipedia.org/wiki/SQL) and [databases](http://en.wikipedia.org/wiki/Dbms)
1. [Differences](http://stackoverflow.com/questions/1326318/difference-between-different-types-of-sql) between the types of SQL - PostgreSQL, SQLite, MySQL
2. [Comparison on RDMS](http://en.wikipedia.org/wiki/Comparison_of_relational_database_management_systems)
3. [MySQL data types](http://www.tibetangeeks.com/technologies/database/datatypes-quickref.html)
4. Using with [php](http://www.php.net/manual/en/mysql.examples-basic.php) and [sinatra application with active record](http://recipes.sinatrarb.com/p/models/active_record)


#Things to learn with SQL

##1. install MySQL

1. install [MAMP](http://www.mamp.info/en/index.html) which comes with MySQL and start the servers
1. open the MAMP's start page to know the connection parameters

    ```
    Host	    localhost
    Port	    3306
    User	    root
    Password	 root
    ```

1. log in to MAMP's MySQL as the root in the command line

    ```
    $ /Applications/MAMP/Library/bin/mysql -u root -p
    Enter password: root
    ...
    mysql>
    ```
1. Install database viewer [Sequel Pro](http://www.sequelpro.com/) and connect in the Standard mode

    ```
    Name:     Learning SQL
    Host:     127.0.0.1
    Username: root
    Password: root
    Port:     3306
    ```

1. clear screen `cmd + k`
1. Quit from the MySQL

    ```
    mysql> quit
    ```

##2. information on databases

1. show all databases

    ```
    mysql> SHOW databases;

    +--------------------+
    | Database           |
    +--------------------+
    | information_schema |
    | mysql              |
    | performance_schema |
    | another_db         |
    +--------------------+
    4 rows in set (0.00 sec)
    ```

1. character sets

     ```
     mysql> SHOW CHARACTER SET;
     ```
1. checkout mysql [data types](http://www.tibetangeeks.com/technologies/database/datatypes-quickref.html)

##2. create and delete database

1. create new database and use it to show that it has zero created tables

    ```
    mysql> CREATE DATABASE computer_networks;
    mysql> SHOW databases;
    mysql> USE computer_networks
    mysql> SHOW tables;
    ```

2. delete database

    ```
    mysql> DROP DATABASE computer_networks;
    mysql> SHOW tables;
    ```

##3. create/read/update/delete data

1. create database

    ```
    mysql> CREATE DATABASE solar_system;
    mysql> SHOW databases;
    mysql> USE solar_system;
    mysql> SHOW tables;
    ```
1. create table 1 on planets and show the table structure

    ```
    mysql> CREATE TABLE planets(
        -> id INT NOT NULL AUTO_INCREMENT,
        -> name varchar(20),
        -> distance float,
        -> mass float,
        -> atmosphere text,
        -> CONSTRAINT id PRIMARY KEY (id)
        -> );

    mysql> DESC planets;

    mysql> SHOW tables;
    ```

1. insert data and show them

    ```
    mysql> INSERT INTO planets(name, distance, mass, atmosphere)
    -> VALUES("Mercury", 0.4, 0.55, "molecular oxygen");

    mysql> SELECT * FROM planets;

    mysql> INSERT INTO planets(name, distance, mass, atmosphere) VALUES("Venus", 0.7, 0.815, "carbon dioxide");
    mysql> INSERT INTO planets(name, distance, mass, atmosphere) VALUES("Earth", 1, 1, "nitrogen");
    mysql> INSERT INTO planets(name, distance, mass, atmosphere) VALUES("Mars", 1.5, 0.107, "carbon dioxide");
    mysql> INSERT INTO planets(name, distance, mass, atmosphere) VALUES("Jupiter", 5.2, 318, "hydrogen and helium");
    mysql> INSERT INTO planets(name, distance, mass, atmosphere) VALUES("Saturn", 9.5, 95.152, "hydrogen");
    mysql> INSERT INTO planets(name, distance, mass, atmosphere) VALUES("Uranus", 19.6, 14, "hydrogen and helium");
    mysql> INSERT INTO planets(name, distance, mass, atmosphere) VALUES("Neptune", 30, 17, "hydrogen and helium");
    mysql> INSERT INTO planets(name, distance, mass, atmosphere) VALUES("Pluto", 48.9, 0.002, "nitrogen, methane and carbon monoxide");

    mysql> SELECT * FROM planets;
    
   | ID |    NAME |        DISTANCE |            MASS |                            ATMOSPHERE |
+-------------------------------------------------------------------------------------------
|  1 | Mercury |   0.40000000596 |  0.550000011921 |                      Molecular oxygen |
|  2 |   Venus |  0.699999988079 |  0.814999997616 |                        Carbon dioxide |
|  3 |   Earth |               1 |               1 |                              Nitrogen |
|  4 |    Mars |             1.5 |  0.107000000775 |                        Carbon dioxide |
|  5 | Jupiter |  5.199999809265 |             318 |                   hydrogen and helium |
|  6 |  Saturn |             9.5 | 95.152000427246 |                              hydrogen |
|  7 |  Uranus |  19.60000038147 |              14 |                   hydrogen and helium |
|  8 | Neptune |              30 |              17 |                   hydrogen and helium |
|  9 |   Pluto | 48.900001525879 |  0.002000000095 | nitrogen, methane and carbon monoxide |
    ```

1. refresh databases in [Seqel Pro](http://www.sequelpro.com/) to see the new database
1. delete Pluto

    ```
    mysql> DELETE FROM planets WHERE name="Pluto";
    ```

1. create and relate another table

    ```
    mysql> CREATE TABLE moons(
        -> id INT NOT NULL AUTO_INCREMENT,
        -> planet_id INTEGER NOT NULL,
        -> name varchar(20),
        -> CONSTRAINT moon_pk PRIMARY KEY (id),
        -> CONSTRAINT planet_fk FOREIGN KEY (planet_id) REFERENCES planets(id)
        -> );

    mysql> DESC moons;

    mysql> SHOW tables;
    ```

1. insert some moon data and show them

     ``` 
    mysql> INSERT INTO moons(name, planet_id) VALUES("Moon", 3);
    mysql> INSERT INTO moons(name, planet_id) VALUES("Phobos", 4);
    mysql> INSERT INTO moons(name, planet_id) VALUES("Deimos", 4);
    mysql> INSERT INTO moons(name, planet_id) VALUES("Io", 5);
    mysql> INSERT INTO moons(name, planet_id) VALUES("Europa", 5);
    mysql> INSERT INTO moons(name, planet_id) VALUES("Ganymede", 5);
    mysql> INSERT INTO moons(name, planet_id) VALUES("Callisto", 5);

    mysql> SELECT * FROM moons;
    
    | ID | PLANET_ID |     NAME |
    +----------------------------
    |  1 |         3 |     Moon |
    |  2 |         4 |   Phobos |
    |  3 |         4 |   Deimos |
    |  4 |         5 |       Io |
    |  5 |         5 |   Europa |
    |  6 |         5 | Ganymede |
    |  7 |         5 | Callisto |
    ```

##4. query/join data

1. play with the data in [SQLfiddle](http://sqlfiddle.com/#!2/d792c6/1)
1. select by columns

    ```
    SELECT name, distance, mass FROM planets;
    ```
1. limit the number of results

    ```
    SELECT name, distance, mass FROM planets LIMIT 5;
    ```
1. order the results

    ```
    SELECT * FROM planets ORDER BY mass;
    ```
1. display moons of each planets

    ```
    SELECT moons.id AS "#", moons.name AS Moons, planets.name AS "Belongs to" 
    FROM moons 
    INNER JOIN planets ON planets.id = moons.planet_id  
        
    | # |    MOONS | BELONGS TO |
    +----------------------------
    | 1 |     Moon |      Earth |
    | 2 |   Phobos |       Mars |
    | 3 |   Deimos |       Mars |
    | 4 |       Io |    Jupiter |
    | 5 |   Europa |    Jupiter |
    | 6 | Ganymede |    Jupiter |
    | 7 | Callisto |    Jupiter |
    ```
  
1. display each planets and then their moons

    ```
    SELECT planets.name as Planets, GROUP_CONCAT(moons.name) as Moons
    FROM (planets LEFT JOIN moons ON moons.planet_id = planets.id)
    WHERE moons.name IS NOT NULL
    GROUP BY planets.id 

    | PLANETS |                       MOONS |
    +-----------------------------------------
    |   Earth |                        Moon |
    |    Mars |               Deimos,Phobos |
    | Jupiter | Europa,Callisto,Io,Ganymede |
    ```


##5. connect with applications

### with php

1. start the MAMP server
1. create a file `index.php` in the document root of the MAMP's web server with the code for [php and MySQL extension](http://www.php.net/manual/en/mysql.examples-basic.php) and amend 3 lines accordingly:

    ```
    ...
    $link = mysql_connect('localhost', 'root', 'root')
    ...
    mysql_select_db('solar_system')
    ...
    $query = 'SELECT * FROM planets';
    ...
    ```

### with ruby, sinatra

1. install [mysql2 gem for MAMP's MySQL with Ruby 1.9.3](https://gist.github.com/jakebellacera/3429066)
1. create a file `app.rb` to create a [sinatra application with active record](http://recipes.sinatrarb.com/p/models/active_record)

    ```
    require 'rubygems'
    require 'sinatra'
    require 'mysql2'
    require 'active_record'

    ActiveRecord::Base.establish_connection(
      :adapter  => 'mysql2',
      :host     => 'localhost',
      :database => 'solar_system',
      :username => 'root',
      :password => 'root',
      :socket => '/Applications/MAMP/tmp/mysql/mysql.sock'
    )

    class Planet < ActiveRecord::Base
    end

    get '/' do
      @planets = Planet.all()
      erb :index
    end
    ```
1. create a file `views/index.erb`

    ```
   <% for planet in @planets %>
      <p><%= planet.name %> at <%= planet.distance %>AU from the Sun with weight of <%= planet.mass %> Earth Mass and mostly <%= planet.atmosphere %> in the atmosphere.</p>
   <% end %>

    ```
1. go to the command lin in the root of the file `app.rb`

    ```
    $ ruby app.rb
    ```



#More Resources on SQL
1. [SQL Essentials screencast course](https://tutsplus.com/course/sql-essentials/)
1. [SQL](http://ruby.bastardsbook.com/chapters/sql/)
1. [Learning SQL book by O'Reilley](http://www.amazon.com/Learning-SQL-Alan-Beaulieu/dp/0596520832)
1. [Online SQL - SQLfiddle](http://sqlfiddle.com/)
2. [Visual explanation of SQL joins](http://www.codinghorror.com/blog/2007/10/a-visual-explanation-of-sql-joins.html)
1. [PostgreSQL, SQLite, MySQL compared](http://zaiste.net/2012/08/db_101_postgresql_mysql_sqlite_compared/)
1. [When to use SQLite](http://www.sqlite.org/whentouse.html)

#Build Link of this episode

[Google I/O 2013 videos](https://developers.google.com/live/)
