## different layer

User request -> controller -> service -> repository (Dao) -> DB

Controller - get info for request and send it back

Service - business logics for processing data

Repository - DAO (data access object) layer - responsible to interact with db and get the data 

use package for different things, 1 package for all models, 1 for service, 

@Service on top of service class. similar to @Component

Repository - a class to do CRUD, operating with db. @Repository similar to @Component

```
all obj models are in model folder
package/models

# package/service
package com.example.demo.service;

import com.example.demo.model.Laptop;
import org.springframework.stereotype.Service;

//Service is similar to @Component
@Service
public class LaptopService {
    public void add(Laptop laptop){
          repo.save(laptop);
    };

    public boolean isGoodForProg(Laptop lap){
        return true;
    };
}

# package/repo
package com.example.demo.repo;

import com.example.demo.model.Laptop;
import org.springframework.stereotype.Repository;

@Repository
public class LaptopRepository {
    //this interacts with db - crud
    public void save(Laptop lap){
        System.out.println("save to db...");
    };
}



# main app
package com.example.demo;

import com.example.demo.model.Alien;
import com.example.demo.model.Laptop;
import com.example.demo.service.LaptopService;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ApplicationContext;


@SpringBootApplication
public class DemoApplication {

	public static void main(String[] args) {
		ApplicationContext context = SpringApplication.run(DemoApplication.class, args);
		LaptopService service = context.getBean(LaptopService.class);
		Laptop laptop = context.getBean(Laptop.class);
		service.add(laptop);
//		Alien obj = context.getBean(Alien.class);
//		obj.code();
//		System.out.println(obj.getAge());

	}

}


```

# Spring jdbc
normal jdbc takes so much time and steps - load driver, connection etc.

We can use spring jdbc to speed up this process.

JDBC templating can u help u to 
1. connect with db
2. fire queries
3. process data
4. get output

DBMS each has its unique features, so we need different drivers for each. H2 in memory db, we will use this
SpringJDBCEx
add dependencies JDBC API
h2 database

## how to map class to a table
class is similar to a table. Student class has attributes similar to cols in the table.

WHat is JPA? 
Java persistence API - bridge between java obj and relationship db.
JPA allows to map java obj into db tables, performing CRUD operation

## JDBC template - use this template to save data
execute(sql, parameters 1, 2, 3)
findall() is to fetch all data

## create schema and data file
In resources folder, u can crate schema.sql and data.sql.

## what is row mapper? -> to map each row into sth
fetch data from your result set, which has all data 1 by 1
rowmapper will get data from result set.
rowmapper is a functional interface

mapRow(Resultset, row no) -> mapRow will get 1 row of data at a time

## how to use external db, instead h2 in memory db 
h2 jdbc knows quickly how to connect without configuration

how to connect from h2 to postgres? H2 is just a transition test db before connected to a real db
U will need to change pom xml, we are not using h2 dependencies (remove this)
we need to insert postgres sql jdbc driver

Then we need to configure datasource url, username, password and driver. Go to resource -> application.properties (spring will use data here when loading)

source: https://github.com/navinreddy20/spring6-course/tree/f1161e151ea06b68810b3cae03005d4955a96a8b/5%20Spring%20JDBC/4.7%20Spring%20Jdbc%20Postgres

