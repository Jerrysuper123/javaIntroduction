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
SpringJDBC

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


 # configure bean using spring xml

 ## object creation
 The moment u r creating an ApplicationContext, the class mentioned in the xml is created.

 If you mention same class/bean 2 times in xml, spring will create 2 objects of the same class.

in xml, you do not have to mention Id, it will still create.

## scope of spring
Even if u getBean 2 times, 1 object will be created. 

When u create the 2 objects from the same bean, both objects refer to the same object. meaning if u update the attribute of 1 object, the other one will be affected too.

in spring, there are 2 types of scope
1. singleton - by default, spring bean is singleton. getBean only create 1 object, even if u called it twice. But singleton will create obj auto when u load xml file.
2. prototype - this will create a new object every time u use getBean. However, this will not create an obj automatically when you load xml file.
 
## setter injection
how to dependency inject into setter, we can do it in xml file.

u can use <property> in the bean. this will call the setter method to reset the value.

## ref attribute
property only work for primitive value, what about obj or ref attribute

in xml, we can use ref attribute for inject an object.
```
<bean id="alien" class="org.example.Alien">
    <property name="age" value="23"></property>
    <property name="laptop" ref="laptop1"></property>
</bean>

    <bean id="laptop1" class="org.example.Laptop" scope="singleton">
    </bean>
```

## constructor injection
how to inject into constructor parameter in xml. Use <constructor-arg value="21"> in xml.

what if u have 2 constructor parameter?
<constructor-arg value="21">
<constructor-arg ref="laptop1">

```
<bean id="alien" class="org.example.Alien">
<!-- value is a primitive type, ref is the object; the order of constructor is important -->
    <constructor-arg value="44"/>
    <constructor-arg ref="laptop1"/>
<!--    <property name="laptop" ref="laptop1"/>-->
</bean>
<!--    default scope is singelton-->
    <bean id="laptop1" class="org.example.Laptop" scope="singleton">
    </bean>
</beans>
```

we can also can type to the const args, so they constructor knows which value to pick up

```
<bean id="alien" class="org.example.Alien">
    <constructor-arg type="org.example.Laptop" ref="laptop1"/>
    <constructor-arg type="int" value="44"/>
<!--    <property name="laptop" ref="laptop1"/>-->
</bean>
```

what if we have 2 int parameters. it would be hard to know using above. We can specify index num for the parameters

```
<bean id="alien" class="org.example.Alien">
    <constructor-arg index="0" ref="laptop1"/>
    <constructor-arg index="1" value="44"/>
</bean>
```

we can also use name as the parameter name
```
    <constructor-arg name="laptop" ref="laptop1"/>
    <constructor-arg name="age" value="44"/>
```

Use constructor annotation to specify the name 
```
<bean id="alien" class="org.example.Alien">
    <constructor-arg name="age" value="44"/>
    <constructor-arg name="laptop" ref="laptop1"/>
</bean>
se
//constructor of Alien class
   @ConstructorProperties({"laptop","age"})
    public Alien(Laptop laptop, int age) {
        System.out.println("multi para constructor called");
        this.laptop = laptop;
        this.age = age;
    }

```

## autowire by name - link auto wire by name
```
<!--    autowire by name - in alien class look for com name to match the bean below-->
<bean id="alien" class="org.example.Alien" autowire="byName">

</bean>
    <bean id="com1" class="org.example.Laptop">
    </bean>

<!--    will look for com var to match the com id here-->
    <bean id="com" class="org.example.Desktop">
    </bean>
</beans>

//in Alien class setter
    public void setCom(Computer com) {
        this.com = com;
    }

```

you can also autowire by type - look for type, not name
```
//it will match by type in the bean,
<bean id="alien" class="org.example.Alien" autowire="byType">
```

## primary bean - there are 2 beans, making it primary, so that spring know which one is primary bean

```
<!--    autowire by name - in alien class look for com name to match the bean below-->
<bean id="alien" class="org.example.Alien" autowire="byType">
    <property name="age" value="44"/>

</bean>
<!--    set com1 as primary, both com1 and com are of the same computer type-->
    <bean id="com1" class="org.example.Laptop" primary="true">
    </bean>

    <bean id="com" class="org.example.Desktop">
    </bean>
</beans>
```

## lazy init of bean
- xml will always create obj no matter what, because of singleton scope
- I do not want to load desktop obj by default by using `lazy-init="true"`
- only when u getbean, the object will be created
- a non lazy bean depending on the lazy bean, the lazy bean will still be created

```
    <bean id="com" class="org.example.Desktop" lazy-init="true">
```

## get bean by type
- getBean only return obj, then we need to do typecasting
- can we get the type of the class
- we can fill in the second parameter of the getBean, to tell its type "Alien.class"
- if u also do not specify the name of the bean, but type only
- getBean(name of bean, type of class)

## inner bean
- focus on laptop only, Alien depending on laptop
- laptop bean for entire app
- we can make laptop bean only responsible for ALien bean only, making it an inner bean or nested bean

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

# spring web app

servlets - serve let => to build server to send bk data
We cannot use jVM to contain server, we need a servlet container or web container. the best option is tomcat.

Besides servlet, another option is reactive. but most is servlet.

Spring boot mvc using servlet

if u build console app, use .jar. if u want to build web app, using .war, built on top of Apache tomcat server???

Embeded tomcat in your project - run project in that tomcat. 

## springboot web
Quick start project. for servlet, add 2 dependencies
1. add servlet - java servlet api 4.0.4
2. embeded tomcat - tomcat embeded core 8.5.96


## running tomcat
create a class for servlet extends HttpServlet

create service(req, res) method

localhost:8080 //tomcat

## servlet mapping
in the past, we map this in xml.

@Webservlet("/hello) -> request then call method below

Source: https://github.com/navinreddy20/spring6-course/tree/f1161e151ea06b68810b3cae03005d4955a96a8b/6%20Spring%20%20Boot%20Web/5.5%20Responding%20To%20The%20Client

## intro to MVC

JSP - jakarta server pages (view tech), put java alongside html

request -> servlet get data -> jsp -> client

MVC
- model - object which has the data
- view - go to client UI 
- controller - handle request, send model data to view tech

CMV - controller (servlet), model (POJO simple java class), view (jsp)

tomcat is server, also servlet container which contains the servlet, JSP will be convert into servlet as well.

## create spring boot web app
spring boot web vs. spring mvc
- spring mvc needs more configuration

3.2.0 version

springBootWeb1

what is packaging?
War - format to run on tomcat

Jar - how to run on tomcat??

add spring web for restful api, use tomcat as defaulted embedded container

run localhost:8080

## creating a jsp page
create a index.jsp file in main/webapp folder

## create a controller
create a HomeController.java in the package
@Controller to annotate this is a controller

## request mapping
@RequestMapping("/) -> pass the path so that we know user is requesting this url

We have to convert JSP into servlet, using Tomcat Jasper, same version as tomcat

source:" https://github.com/navinreddy20/spring6-course/tree/f1161e151ea06b68810b3cae03005d4955a96a8b/6%20Spring%20%20Boot%20Web/5.10%20RequestMapping

## send data to controller 
Add a form and button to submit

## accept data the servlet way
Dispatch servlet - this is a request, call this method

there 2 ways to accept data:
1. servlet way
2. spring boot way

## display data on result page
get http session and then pass the value into the session.

session.setAttribute(key, value);

Weird to use session to add data.

## request param
instead of using session, we use the parameter from url.

int num1 -> will get ?num1=9 from the url

@RequestParam("num1") int num1 -> get the value from para and assign to num1

## model project

Model model, we can use it instead of session

model.addAttribute() //use to transfer data between controller and jsp

remove .jsp extension and add a view folder

## set prefix and suffix
View resolver has do the url mapping and return the view

We can config in the Application.properties

prefix = .view folder
suffice = .jsp file extension

## model and view
put css file outside webapp

you can also put the css file in static file

ModelandView mv.addbject("result", result);
mv.setViewName("result") //jsp file

## model attribute
when we capture the data from url parameters

we need a model/class to contain them - Alien class

can we accept one particular object, instead of 1 by 1 variable

we use model attribute to solve this problem
use @ModelAttribute
- this is optional to put
- we can also can this on top of method to inject into jsp as a model

source
https://github.com/navinreddy20/spring6-course/tree/f1161e151ea06b68810b3cae03005d4955a96a8b/6%20Spring%20%20Boot%20Web/5.19%20Using%20ModelAttribute

# spring mvc

this is to do MVC on spring boot, easier.

But spring mvc is more diffcult in terms of configuration.

in spring mvc, we will use external tomcat, instead of embeded tomcat.

use elipse to run the server

tomcat 9 use javax, 

choose spring webapp

- add spring mvc
- point controllers to tomcat
- dispatcherservlet - help tomcat to talk to controller
- internval view resolver - help to point to the right view jsp files

# section 13: building a job portal

what is lombak? help to reduce no of lines of the code

JobApp
add spring web and lombok in spring initializer

Add Jasper to convert jsp pages (nobody does thi nowadays) into servlet to be used - everyone move to React.

JSP is outdate, no one is using it nowadays. 
@PostMapping for handle forms

Lombok helps to reduce no of lines of codes
- @Data -> no need to create ds, no need to create toString, getter and setter
//for constructor we need to put below
- @NoArgsConstructor
- @AllArgsConstructor


Focus on GetMapping and PostMapping

We need to save job in job service layer.
- one to view data
- one to add data
We also need a repo layer to interact with db

DTO - data transfer object

What is model, service and repo?
mode is the basic structure of the data object - model the data structure

Service is to serve the data like a service, it can contain the business logics and wrap repo

Repo connects with the db directly, and does not contain any business logics


Source: https://github.com/navinreddy20/spring6-course/tree/313724977c6a0c76231119984893c89923591d5d/8%20JobApp-Project

# section 14 Rest using spring boot

## rest - rep state transfer
send state (job) to client, then we transfer it to client

stateless - every request is stateless, client needs to tell server who they are

## http
PutMapping
DeleteMapping

## build a rest controller
@Controller assumes u r returning jsp view

add @ResponseBody to tell that u r returning a response body instead of a view

you can use @RestController to tell spring it is a rest, so that you do not have to specify @ResponseBody

at backend we have to allow crossOrigin React UI to fetch this.
@CrossOrgin=(orgins="http:localhost:3000")

## path variable - url variable jobPost/3
how to get 3 from the 

@GetMapping("jobPost/{postId})

@PathVarible("postId) int PostId using this annotation

## @RequestBody JobPost jobpost
annotation to tell this is a job post

## @PutMapping and @DeleteMapping

## Content negotiation
can we get data from postman in xml format

We need to adjust this in server.

Jackson convert java obj into json

for xml, add jackson with xml support. DataFormat xml

Can we restrict server to send only json? produce
@GettMapping("jobPosts", produce={"application/json"})

//accept only xml on server
//server consume xml only
@PutMapping("jobPosts", consume={"application/xml"})

source:
https://github.com/navinreddy20/spring6-course/tree/36d783117044564847940ba5046b35cc59f55346/9%20REST%20using%20Spring%20Boot/8.10%20Put%20and%20Delete%20Mapping/spring-boot-rest4

## section 15 spring data JPA

Easier to work with db is spring JPA

Earlier we used JDPC template, where we have written sql query

in JPA and ORM, we can avoid writing sql.

ORM - object relationship mapping

Can we create a table based on a className, class attributes based on the class definition. In this way, we can map our class to table.

The most famous tool is hibernate, to map class object into table. every object becomes a new row in the table.

Java persistency API. If u use hibernate ORM, u need to write a lot of code. But with spring data JPA, you can save some code writing.

## create table and inserting data

add dependencies Spring data JPA (it uses hibernate behind the scenes). PostGres sql driver.

create a new JPA repository. Class extends JpaRepository<Student, Integer>

@Entity to Student class, so that it is a class to map to table

@Id on top of roll no as the primary key

//We can do this even though JpaRepository is an interface
repo.save(s1);

//in application.properties
//if there no table, create
//if there is table, update only
spring.jpa.hibernate.ddl.auto=update
//this will show sql in terminal command line
spring.jpa.show-sql=true

## findall
similar to sql query select * from student;

## findbyId
simlar to select * from student where rollId=3;

```
Optional<Student> s= repo.findById(103);
	System.out.println(s.orElse(new Student()));
```

## query DSL (domain specific language - where the query will be created based on your method name, unstable)
search by name

do we have findByName, we have to create a function in repo

@Query annotation on top the abstract method

Write JBQL 

if we do not provide query annotation, it will will still work.

MethodName has to be in the format of findByMarks, findByName, findByMarksGreaterThan

## update and delete
you can use .save to update the db

repo.delete -> to delete a row

SQL vs. ORM in Terms of Performance
SQL (Native Queries): Writing raw SQL queries can give you the most control over performance. With SQL, you can optimize queries and ensure they are as efficient as possible, especially in complex operations. If your application needs to perform complex, highly optimized queries, raw SQL may offer better performance.

ORM: ORM abstracts the database interaction, which is very convenient and reduces boilerplate code. However, ORMs typically generate SQL queries automatically, which can sometimes be suboptimal. However, with careful tuning, caching, and proper use of annotations, you can mitigate many of the performance issues associated with ORM.

## convert job app into JPA
add JPA and postGres

## search by keyword
@GetMapping("jobPosts/keyword/{keyword})

//JPA based on the method name below create sql query
findByPostProfileContainingOrPostDescContaining()

# section 15: project using spring boot mvc

## create a backend for the product app
spring web
postgres
lombok

dependencies

/api/products
To have a general mapping
we can @RequestMapping("/api) before the class, then @GetMapping("/products") before method

## create product model and table

@Id
@GeneratedValue=(strategy=GenerationType.IDENTITY)

auto create primary key


## response entity
from server side, we can send the status code

200 - success
201 - created, 202

400 client error
401 unauthorized

500 - server side error

this is called responseEntity<List<Product>>

this is a class to create resp results and http code status
## fetch product by ID

//use ifElse to avoid null pointer
return null.ifElse(new product(-1))

//use below pattern to format date
```
@JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "dd-MM-yyyy")
    private Date releaseDate;
```

The @JsonFormat annotation is used in Java to specify how a particular field should be serialized or deserialized when working with JSON data, typically with frameworks like Jackson. Jackon is doing serializing and deserialization??

## add product with image

@Lob
private byte[] imageData

//not sure what
ResponseEntity<?>

postman - form data, value and contenttype to send image

The @RequestPart annotation in Spring MVC is used to handle multi-part form data, especially when you have a combination of different types of inputs in a single request, such as:

A Java object (Product in your example) serialized as JSON or XML.
A file upload (e.g., an image, document, or other binary files) represented as MultipartFile.

@RequestPart Product product:

This maps a specific part of the multi-part request (typically JSON or XML data) to the Product object.
The part of the request corresponding to this field should be deserialized into the Product object by Spring automatically.
@RequestPart MultipartFile imageFile:

This maps another part of the multi-part request (binary data) to a MultipartFile object.
This is used for handling uploaded files, allowing access to the file's content, metadata, and other properties.

## update and delete product


## search product
//using jbql (class name) to map the sql function
@Query("jbql")
searchProducts

JBQL (Java Persistence Query Language) is a query language used in Java Persistence API (JPA) to interact with relational databases. JBQL is similar to SQL but is specifically designed to work with JPA entities rather than directly interacting with database tables.

```
// -> request /product/search?keyword=samsung
@GetMapping("/product/search")
    public ResponseEntity<List<Product>> searchProducts(@RequestParam String keyword) {
        List<Product> products = productService.searchProducts(keyword);
        System.out.println("searching with :" + keyword);
        return new ResponseEntity<>(products, HttpStatus.OK);
    }
```

https://github.com/navinreddy20/spring6-course/tree/main/16%20Project%20using%20Spring%20Boot%20MVC/16.11%20Search

Mine - 16.5 fetching all products from DB

## spring data rest
JPA handles all the sql queries.

1. controller
2. service
3. repository

Can we remove service layer? 
- we can remove service and controller layer also, only leaving the repository layer
- very abstract

## create a data rest project

add dependency
- rest repository
- spring data jpa
- postgre driver

## running the project
/jobPosts will work

if u will also auto seen some href to get a specific  job
href /jobPosts/1

these controller are built by repo and fit into the return result

## update and delete
you can use the same href /jobPosts/1 to update and delete the data

https://github.com/navinreddy20/spring6-course/tree/74da051ec4aeba288e50b533624ed20451d02a57/11%20Spring%20Data%20Rest/10.4%20Update%20And%20Delete/spring-data-rest-demo

# Section 18. Spring AOP
## spring AOP intro
Aspected oriented programming

what is the problem of cross cutting concern?

this is to emit the metrics - and logging files

we need to have for each controller function
- log
- security analysis
- validation
- exception

We need to write a lot of lines into the function, this will crowd the function.

Separation of concern 

create separate class for logs, for every method in the controllers. How are we going to call that method.

We want to make that we do not have to call it to make code hard to read.

## logging the calls
Create a LoggingAspect class in aop folder

for every method called, we want to maintain a log of it.

```
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;


	
public class LoggingAspect {

	public static final Logger LOGGER=LoggerFactory.getLogger(LoggingAspect.class);
	
	

	public void logMethodCall() {
		LOGGER.info("Method Called");
	}
}
```

## AOP concepts


joint point - a scene in the movie, where action happens

advice - this is the action e.g. update or delete method

Aspect - script of movie, defines plot twists (advice) and where (Pointcut)

pointcut - where conceptually - a specific scene where action occcur. it is like bookmark of your script
 
target - it is the object that experiences the actions

weaving - this is director job, how script is turned into a movie. in sping aop, this happens at runtime.

proxy - it is the stun double that makes the main character look cool.

type of advice
- before
- after
- after throwing 

hard to understand

## before advice (action)
@Component
@Aspect

//we have to mention all below
//return type, className.method-name(args)

//before advice
//* is wild car
//.. for all args
@Before("execution(* *.*(..) )")

```
@Aspect
@Component
public class LoggingAspect {

	public static final Logger LOGGER=LoggerFactory.getLogger(LoggingAspect.class);
	
	

	@Before("execution (* com.telusko.springbootrest.service.JobService.*(..))")
	public void logMethodCall() {
		LOGGER.info("Method Called ");
	}
}
```


## joinPoint - 
specify update job only

JoinPoint will give us the method signature of the method

## after advice - after action

@After = after finally

@AfterThrowing = will perform only if there are exceptions

@AfterReturning = after function is exe success

we are observing the app using external class, also separate this observation from working class.

## around advice - at start, middle and at end.

create a PerformanceMonitorAspect class

end time - start time = time used to call the method

in between around, we have to execute the method

## validate input using around advice
lets say user send jobPosts/-4, we do have a -4 value.

Before hitting actual service, we can validate the input value in AOP as well.


Source: https://github.com/navinreddy20/spring6-course/tree/11f237bb932acf93f531fa278b8bad20d92a2304/12%20Spring%20AOP/11.8%20Validating%20The%20Input%20Using%20Around%20Advice/spring-boot-rest

# Section 22 docker
pull - is pulling image docker hub
run - run the image to become the container


```
docker --version

//this will pull image also
docker run hello-world
docker pull hello-world

//list of container
docker ps -a //see all container existed before
docker ps

//remove container
docker rm <containerId>

//list images
docker images
docker rmi <imageId>


//start an image
docker start <imageId>
search search <hello-world>

//docker run interactive mode
docker run -it openjdk:22-jdk
```

## Docker architecture
user -> docker client -> local docker -> remote registry

Docker
- daemon - get image and run container
- images
- container
- network
- volumne

## packing the spring boot web app
in pom.xml below, final name will bt eht final jar file.

```
<build>
    <finalName>rest-demo</finalName>
</build>
```

click on maven, mvn package
 to get rest-demo.jar

jar is executable jar

//to run below
//verify it is working
java -jar target/test-demo.jar

## run spring jar on docker
```
docker exec <containerId> ls -a

//we can put in temp
docker exec <containerId> ls /tmp

//take jar and put into the /tmp folder

//cp file from local to container
docker cp target/rest-demo.jar <containerId>:/tmp

//create an image
docker commit <containerId> <imageName>:tagNo

//we do not want default command jdk to be jshell

//to get around this 
docker commit --change='CMD ["java","-jar","/tmp/rest-demo.jar"]' <containerId> <imageName>:tagNo

//expose container port
docker run -p 8080/80801 <image>:tagNo
```

## docker file and docker images
many steps for above

that is why we need Dockerfile

```
Docker build -t rest-demo:v3 .
```

DockerFile
```
FROM openjdk:22-jdk
ADD target/rest-demo.jar rest-demo.jar
ENTRYPOINT ["java","-jar","/rest-demo.jar"]

```

## web app with postgres
how to run multiple container
1 for server and another for db

create a new project

when u have jpa dependency, if u did not set up db, you app will not run.

app properties, for sql to work in docker, we will need below
```

```

## docker compose
use DockerFile to dockerize the app

Docker compose is used to work with multiple containers with just 1 command

create a docker-compose.yml file

```
docker compose up --build
```

docker-compose.yml
```
version: "3.7"

services:
    app:
        build: .
        ports:
            - "8080:8080"
        networks:
            - s-network

    postgres:
        image: postgres:latest
        environment:
            POSTGRES_USER: navin
            POSTGRES_PASSWORD: 1234
            POSTGRES_DB: telusko
        ports:
            - 5433:5432
        networks:
            - s-network
        volumes:
            - postgres-s-data: /var/lib/postgresql/data
networks:
        s-network:
            driver: bridge
volumes:
    postgres-s-data:
```

## after running above, there are 2 ports, how do they communicate with each other
How to run multiple containers

change application properties attribute to align with docker compose.yml

how to skip test when run mvn build
```
mvn clean package -DskipTests
```

first turn down the compose before up again
```
docker-compose down
```

list network
```
docker network ls
```

## volume
if we add data, we restart container, we will lose value. But we can use container to store these data.

create a addStudent function to generate a datapoint for the database, but it will not always be there because data.sql does not contain the script

we can use volume to store this
```
volumes:
    postgres-s-data:
```


source: https://www.udemy.com/course/spring-5-with-spring-boot-2/learn/lecture/40722494#overview


# section 23 cloud computing
you also need admin to handle the server and db, configuring the system. Cloud can help on this.

elastic cloud computing

on premises 
- functions
- applications
- runtime and containers
- OS and mangement tools
- networking, storage and servers
- data centre

infra as a service - if i only want to manage OS and software, then we for infrastructure as a service. then we do not need handle data centre and networking stuff.

Container as a service, i do not even want to manage OS.

Platform as a service, i do not event want to manage container

serverless computing - function as a service. I only want a function to do something

software as a service - google photo. Online IDE.

cloud also has cost and privacy issues.

U can create a private cloud - somebody manage this, but cost will be higher.

hybrid cloud - some data is public and some is private cloud


# section 24 micro services
monolithic vs. micro services

scalability is hard for monolithic; u only need to scale on search instead of the entire application

individual service, if one service is down, the other services will stil be up.

use API gateway for each service to communicate with each other

## cloud native
most companies moving from onpremise to cloud.

cloud-ready - u have an app on premise, u have to make some changes to the existing app to make it cloud-ready

cloud-native - build a new app on cloud directly and natively using all its features

follow 12 factor app to be cloud native
1. code base - has version control
2. dependencies - declare explicitly, keep it separate
3. config - store config (port) in the environment. env. to save all in one place
- so that it can save in one place

do not hard code the configuration - configure out side the code
4. backing service
5. build -> release -> and run
strictly separate build and run stages

always separate build and release. build has a release version, so that u can revert to previous versions
6. processes - do not store the data in the process, data must come from the attached db
7. port binding - every service has a different port
8. concurrency - scale out
9. disposability - easy to dispose a service, should be easy, but should not lose data
10. dev prod parity
keep dev, staging and production env as similar as possible

use docker image to resolve this issue
11. logs - treat logs as event streams
12. admin process
should admin ur app from outside

need to follow the 12 steps to be cloud native

## quiz app
move from monolithic app to micro services

CRUD on questions

add dependencies, postgres, jpa, spring web, lombok

connect to db with many questions db

1. Controller
2. service layer
3. DAO - data access object

In Java, DAO stands for Data Access Object. It is a design pattern used to separate the data persistence logic from the business logic in an application. By using a DAO, you create an abstraction layer that handles operations with a database or other data storage systems.

Purpose of DAO Pattern:
Separation of Concerns:
It separates the database access logic from the main application logic.
Code Reusability:
Data access code can be reused across different parts of the application.
Maintainability:
Changes to the database structure or logic can be handled in the DAO layer without affecting business logic.
Testability:
DAOs can be easily mocked or tested independently of the rest of the application.
How DAO Works:
The DAO acts as an intermediary between the application and the data source (e.g., a database).
It provides methods to perform CRUD operations (Create, Read, Update, Delete) on the data.
Typically, it uses JDBC, Hibernate, or other persistence frameworks for database interactions.


## quiz app - set up part 3
when u have to custom sql in jpa - use use
1. HQL
2. JPQL - jpa query language

however, if u rename the method name properly, by findByCategory in interface QuestionDao, jpa auto knows u r finding Category col in the table.

## quiz app - part 4
add responseEntiy object instead of List<Question>

## quiz app - part 5
create quiz - fetch java question by difficulty

```
/quiz/create?category=Java&noOfQuestions=5&title=Jquiz
```

## quiz app - part 8
monolithic app
https://github.com/navinreddy20/quiz-app-spring


## build micro services from monolithic
question, quiz, and user, and certificate -> they all run independently

question will has it own db
service will has its own db
but we have to connect them together using http

API gateway - user initiated from a single API, then redirected to different services through API gateway

Load balance - when one service is calling another, we will use load balancer to redirect

Service registry - who is calling who, we need to a way to manage this

failed fast - if one of the services return null, it should fail fast to notify another service

## build a question microservices
create artifact: question-service
- spring web
- postgres sql driver
- lombok
- spring data spa
- OpenFeign (spring cloud routing)
- Eureka discovery client (spring cloud discovery)

Comment out openFeign and Eureka atm

create separate database for question service

### create question service 2
create a generate function for service to call to get some questions

Dao to return list<Integer> and return only question id only

Create getQuestions - return a list of questions based on wrapper Response, based on a list of question id

create getScore - calculate the final score value and return to quiz-service

## running question service
we might have multiple instances of question service, not just 1 server

testing in postman

how to run multiple instances of the same service
- Go to Edit configuration
- copy configuration
- instance 9091
- add vm options
- "-Dserver.port=8081"
- you will see 2 options of instances running on the same service

## create quiz service part - 4
create API points
- createQuiz
- get
- getscore

Create an artifact quiz-service

create a new db "quizdb" in page admin
- table auto-generated by the model jpa
- we need to keep all models

createQuiz can accept 1 big object instead of 3 separate request params - QuizDto (data transfer object)
- create QuizDto class

for quiz-service to call question-service, we have to use rest template to init api call

we cannot use localhost and port number, because we do not why which instance, localhost can have multiple instances. so instead of rest template, we can use other methods
- we need openFeign to configure the call
- we also need service discovery - quiz service has to discovery question service
- we can use Eureka client
1. each service will have eureka client
2. when they search for each other, they will go to Service registry/Eureka server to know which one to call
3. the process goes about 1) register and 2) discovery

## create a service register
service x (with Eureka client) -> reach to Service register/Eureka server -> service y (with Eureka client)

in the process of register and discovery, bi-directional

create a new artifact -> service-registry, add dependencies
- spring web
- Eureka server

We need to change
- Add @EnableEurekaServer in app
- change app properties

when u load eureka server, this will create a dashbooard, showing which instances are connected to the server.

let us connect question-service to service-registry
- go to POM file to uncomment eureka client
- go app properties to add a name to be discovered by Eureka server


Next, for quiz-service to discover question-service, we will use Feign to do this.

-- very interesting

## using feign
in quiz service, when create quiz, when we want quiz-service to interact with question-service.

instead of rest template (port and url), we use feign to make it short easier.
- feign will search the service for you

create a feign package inside quiz-service.
- create a file inside feign package, enable quiz to connect to question service
- enable eureka client and feign in quiz-service
- in quizinterface, connecct to question-service

update Quiz.java
- change many to many to elementCollection
- change to questionIds

At application level
- enableFeignClients

change quiz-service to port 8090 number

## microservice is calling microservice

## completing the 2 microservices
Complete quiz-service other 2 APIs

## load balancing

if u have multiple instances of the same app.
if one instance is busy, we can call the other instances

when quiz service is calling question-service
- we can push the api call to the direct instance with low load
- spring has load balancing
- the moment u r using feignclient, it will use the service that has lower load
- get Environment bean, Environment.getProperty("local.server.port"); this tells us which port it is running

## API gateway
Users has not interest in how your microservice is working; user only need 1 API service to interact with all services.
- user do not need to log in in each services to use the product
- in spring cloud, we get API gateway automatically

create an api-gateway artifact, adding dependencies
- gateway
- Eureka discovery client

a client send a request to API gateway, then api gateway find the right services and API to call

source: https://github.com/navinreddy20/MicroserviceTutorials



# section 24 micro services



