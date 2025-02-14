# Maven - project manager for external lib
make sure all versions are in sync
spring will work with hypernate?? hypernate is just an ORM - wrapper for db.

Similar to Maven, there are other project manager like Gradle and ivy

# maven has lifecyles to manage build, test etc and plugins as well

# Maven archetypes - templating tools
they are templates for us to start

Dependencies are in Jar files
- Jar is java archive file
- Why do we pack everything in jar file?
1. standard way to pack all classes, resources and metadata into a single file. Single file is easy to distribution
2. jar is essentially like zip file, compressing the file size
3. Tools like Maven and Gradle manage dependencies in Jar format

Go to mvnrepository.com to try mysql connector

# what is GAV - group id, artifact and version
group id = unique reversing of your domain e.g. com.oracle
artifact = project name
version = project version

We need the GAV to create a jar file

```
//top level
<dependencies>
<!-- https://mvnrepository.com/artifact/com.mysql/mysql-connector-j -->
<dependency>
    <groupId>com.mysql</groupId>
    <artifactId>mysql-connector-j</artifactId>
    <version>9.1.0</version>
</dependency>
</dependencies>
```

# transitive dependencies
- i do not want the dependency, but my lib wants it
```
Your Project
   ├── Depends on: Library A
          ├── Depends on: Library B (Transitive Dependency)
```



# POM (project object model _ XML rep of a MAVEN project) -> show Effective POM
- there are more thing in Effective POM. 
- There are plugins to be used, although it does not show up in POM
- developers just modify POM, framework will create necessary effective POM

- POM or (pom.xml) is the one used by developers
- while effective POM is the actual config Maven used after applying all defaults and inheritance

# Maven Archetype - existing templates
meaning of archetype in english
- an original model or prototype in English

there are too many templates to be used
j2ee, webapp

Maven central has multiple archetype

Usually when we download spring boot, it is already configured. 

# How Maven works - .m2 is the local dependency cache
.m2 folder - you will see the repository
- everytime Maven downloads a dependencies, it will create a local .m2 folder
- if dependency is not there, go to maven central to download
- some company will have their own local repository, if it is not there, you cannot use it for security reasons.


  
# pg admin 4 - admin panel for db
create db demo
db name -> schema -> public -> table -> create table student

sid integer primary key, not null
sname text
marks integer

view and edit table to insert data

Execute the query
```
insert into student values (1, 'Navin', 50);
```

# why we do not concat sql statement with var
1. prevent sql injection
2. cache the sql for multiple queries
3. easy to make mistake with concat

better use prepared statement

prepareStatement(sql) -> prepare the sql statement for var replacement later, it is cache in memory

Prepared statement will convert user input (later part of below sql statement) into a string. and it will not be executed as sql.
```
SELECT * FROM users WHERE username = 'john''; DROP TABLE users; --'
```
# Spring framework 
ORM - hibernate
Spring - light weight
Pojo - plain old java object

spring is an econsystem.

useful for enterprize application

servlet - what is it? what is spring mvc? tomcat - servlet container?? what is it

- servlet handles request and response, just like Express.js. They run on a server
- Tomcat is a web server and servlet container (need a war. file and run inside Tomcat). It is similar to node.js runtime and Express.js combined

# What is a war file?
A WAR (Web Application Archive) file is a packaged Java web application that contains everything needed to run the app, including:

    Java classes (Servlets, controllers)
    JSP files (if used)
    HTML, CSS, JavaScript (frontend)
    Libraries (.jar files)
    web.xml or WEB-INF/ configuration files

Think of a WAR file as a ZIP archive that Tomcat understands and can deploy.

# IOC - inversion of control (a design principle - object creation is shifted from app code to framework or container ) and DI - dependency injection (design pattern)

IOC - inverting the control
- create obj, maintain and destroy the obj, and control the flow
- as a programmer, u are to build biz logics, so IOC takes away those obj creation and flow control, so u can focus on biz logics
- Within a Jvm, there is a IOC container will contain the obj in spring for u

How does IOC principle work?
- we make use of design pattern dependency injection
- we inject the obj directly without creating a new obj ourself

It  is better to use dependency injection to avoid tightly coupled system.
- Client directly depends on Service (new Service() inside Client).
- If we change Service (e.g., rename serve() method), we must modify Client.
- If we want to replace Service with NewService, we must change every place new Service() is used.
```
class Service {
    void serve() {
        System.out.println("Service is running...");
    }
}

class Client {
    private Service service;  // Direct dependency (tight coupling)

    Client() {
        this.service = new Service();  // Object is created inside Client
    }

    void doWork() {
        service.serve();
    }
}
```




# spring vs spring boot
in the past, to use spring, we have to config xml file, create project and create beans, very troublesome. 

To ease this, we use spring boot - opinionated framework.


# Dependency injection in spring boot

any obj created and managed by spring is called bean.

Below start and run a IOC container
```
springApplication.run()
```

ApplicationContext (way to access IOC container) will have all the objects created.

But u must com with IOC which object u want me to create.
- we must use annotation @Component
- so spring knows it will need to manage the obj
- then do the dependency injection

# What is auto wiring in spring boot
@Autowired this is used for a nested class in a @Component class

auto wire for the sub class.

# there are 3 ways to manage the beans
1.xml
2.java-based configuration
3. annotations

Depending on company u r working with. They might use 1 of the above.

Skipped 1 and 2 and jump to 3 for spring.


# JDBC connection - get query
JDBC - java database connectivity, JDBC connect app to database
postgres - relationship database management

JDBC API - allow java to interact with database

```
/*
* steps to establish jdbc conection
* 1. import package
* 2. load and register driver
* 3. create connection
* 4. create statement
* 5. execute statement
* 6. process results
* 7. close
* */

//import package
import java.sql.*;

import static java.lang.Class.forName;

public class DemoJDBC {
    public static void main(String[] args) throws Exception {
        // jdbc:<db type>://localhost:<port no>/<db name>
        String url = "jdbc:postgresql://localhost:5432/Demo";
        String uname = "postgres";
        String pass = <pw>;
//        String sql = "select sname from student where sid = 1";
        String sql = "select * from student";
        //load driver
        Class.forName("org.postgresql.Driver");
        //establish connection
        Connection con = DriverManager.getConnection(url,uname, pass);
        System.out.println("connection established");
        Statement st = con.createStatement();
        ResultSet rs = st.executeQuery(sql);
        //next row is the actual data, after column header
//        rs.next();
//        String name = rs.getString("sname");
//        System.out.println("the student is: " + name);

        //loop through all rows
        while (rs.next()){
            System.out.print(rs.getInt(1) + " - ");
            System.out.print(rs.getString(2) + " - ");
            System.out.println(rs.getInt(3));
        }

        con.close();
        System.out.println("Connection closed");


    }
}

```

# JDBC insert into db
```
public class DemoJDBC {
    public static void main(String[] args) throws Exception {
....
        String sql = "insert into student values (6, 'John', 49)";
        Class.forName("org.postgresql.Driver");
        Connection con = DriverManager.getConnection(url,uname, pass);
        System.out.println("connection established");
        Statement st = con.createStatement();
        //check if execution has happened
        //it will return false, even though exe success weird
        boolean status = st.execute(sql);
        System.out.println(status);
        con.close();
        System.out.println("Connection closed");

    }
}

```

# update db 
```

public class DemoJDBC {
    public static void main(String[] args) throws Exception {
        // jdbc:<db type>://localhost:<port no>/<db name>
        String url = "jdbc:postgresql://localhost:5432/Demo";
        String uname = "postgres";
        String pass = "Je-112233";
        String sql = "update student set sname='Max' where sid=6";
        Class.forName("org.postgresql.Driver");
        Connection con = DriverManager.getConnection(url,uname, pass);
        System.out.println("connection established");
        Statement st = con.createStatement();
        st.execute(sql);
        con.close();
        System.out.println("Connection closed");


    }
}
```

# delete row
```

public class DemoJDBC {
    public static void main(String[] args) throws Exception {
        // jdbc:<db type>://localhost:<port no>/<db name>
        String url = "jdbc:postgresql://localhost:5432/Demo";
        String uname = "postgres";
        String pass = "Je-112233";
        String sql = "delete from student where sid=6";
        Class.forName("org.postgresql.Driver");
        Connection con = DriverManager.getConnection(url,uname, pass);
        System.out.println("connection established");
        Statement st = con.createStatement();
        st.execute(sql);
        con.close();
        System.out.println("Connection closed");


    }
}

```

# prepared statement, replace ? parameter with values
```
public class DemoJDBC {
    public static void main(String[] args) throws Exception {
       ...
        String sql = "insert into student values (?,?,?)";

        //user input
        int sid = 102;
        String sname = "Jamsine";
        int marks = 45;

        Class.forName("org.postgresql.Driver");
        Connection con = DriverManager.getConnection(url,uname, pass);
        //this cache and prepare the sql statement, for ? to be replaced with parameters
        PreparedStatement ps = con.prepareStatement(sql);
        //replace ? with user inputs
        ps.setInt(1, sid);
        ps.setString(2, sname);
        ps.setInt(3, marks);
        //preparestatement exe
        ps.execute();
        con.close();

    }
}
```
# Spring boot and Inversion of control and dependency injection
```
package com.example.demo;

import org.springframework.stereotype.Component;
//Annotation to tell IOC container to manage this component for us
@Component
public class Alien {

    public void code(){
        System.out.println("coding");
    }
}

package com.example.demo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ApplicationContext;

//annotation tells that this is a spring boot config to use annotation
@SpringBootApplication
public class DemoApplication {

	public static void main(String[] args) {
		//run app and return applicationContext which can access IOC (inversion of control) container - stupid name
		//just say object control container, so hard for people to remember
		//this app context is similar to React context
		ApplicationContext context = SpringApplication.run(DemoApplication.class, args);
		//bean is object, so get beans means creating obj from this class
		Alien obj = context.getBean(Alien.class);
		obj.code();
		Alien obj2 = context.getBean(Alien.class);
		obj2.code();


	}

}

```

# @Autowired - a way to tell IOC container that a subclass is also part of the beans
```
package com.example.demo;

import org.springframework.stereotype.Component;

//Subclass also must have component annotation
@Component
public class Laptop {
    public void compile (){
        System.out.println("compiling...");
    };
}

package com.example.demo;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;
//Annotation to tell IOC container to manage this component for us
@Component
public class Alien {

    //we auto wire below subclass laptop as part of the beans
    @Autowired
    Laptop laptop;
    public void code(){
        laptop.compile();
    }
}

package com.example.demo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ApplicationContext;

@SpringBootApplication
public class DemoApplication {

	public static void main(String[] args) {
		ApplicationContext context = SpringApplication.run(DemoApplication.class, args);
		Alien obj = context.getBean(Alien.class);
		obj.code();

	}

}

```


# use XML to configure beans in a Maven quick start template
Add spring dependencies
```
<!--    //add spring context to get ioc container-->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>6.1.14</version>
    </dependency>
  </dependencies>
```

Configure bean at src/main/resources/spring.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="
        http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

<bean id="alien" class="org.example.Alien">

</bean>
</beans>
```

Alien class
```
package org.example;

public class Alien {
    public void code(){
        System.out.println("coding");
    };

}

```

App class
```
package org.example;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

/**
 * Hello world!
 */
public class App {
    public static void main(String[] args) {
        //applicationContext is a superset of beanfactory interface
        //classpathxml is one way of config context
        //there are other ways as well
        //1. xml
        //2. java basedd
        //3. annotation

        ApplicationContext context = new ClassPathXmlApplicationContext("spring.xml");
        Alien obj = (Alien) context.getBean("alien");
        obj.code();

        System.out.println("Hello World!");
    }
}

```

 ## object creation
 The moment u r creating an ApplicationContext, the class mentioned in the xml is created.

 If you mention same class/bean 2 times in xml, spring will create 2 objects of the same class.

in xml, you do not have to mention Id, it will still create.

```
 //whenever this line is called, beans are created based on xml file
        ApplicationContext context = new ClassPathXmlApplicationContext("spring.xml");
```

## scope
Even if u getBean 2 times, 1 object will be created. 

When u create the 2 objects from the same bean, both objects refer to the same object. meaning if u update the attribute of 1 object, the other one will be affected too.

in spring, there are 2 types of scope
1. singleton - by default, spring bean is singleton. getBean only create 1 object, even if u called it twice. But singleton will create obj auto when u load xml file.
2. prototype - this will create a new object every time u use getBean. However, this will not create an obj automatically when you load xml file.
   
```
<bean id="alien" class="org.example.Alien" scope="prototype">
</bean>
<!--    default scope is singelton-->
    <bean id="laptop" class="org.example.Laptop" scope="singleton">
    </bean>
</beans>
```

## setter injection
how to dependency inject into setter, we can do it in xml file.

u can use <property> in the bean. this will call the setter method to reset the value.

```
<bean id="alien" class="org.example.Alien">
    <property name="age" value="23"></property>
</bean>

package org.example;

public class Alien {
    private int age;

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public Alien(){
        System.out.println("Alien obj created");
    }
    public void code(){
        System.out.println("coding");
    };

}

public class App {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("spring.xml");
        Alien obj = (Alien) context.getBean("alien");
        obj.code();
        System.out.println(obj.getAge()); //23

    }
}
```

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

```
        Alien obj = (Alien) context.getBean("alien", Alien.class);
        obj.code();
        System.out.println(obj.getAge()); //23

        //although we r creating interface of computer
        //a laptop is created
        Computer com = context.getBean(Computer.class);
        com.compile();
        Desktop desk = context.getBean(Desktop.class);
```

## inner bean
- focus on laptop only, Alien depending on laptop
- laptop bean for entire app
- we can make laptop bean only responsible for ALien bean only, making it an inner bean or nested bean

```
<bean id="alien" class="org.example.Alien" autowire="byType">
    <property name="age" value="44"/>
    <property name="com">
        <bean id="com1" class="org.example.Laptop" primary="true">
        </bean>
    </property>
</bean>
```

# java-based configuration
most people prefer java-based config.

## create AppConfig class to manage the beans
Within the same package, create /config/appConfig.class
```
new AnnotationConfigApplicationContext(AppConfig.class)
```

put @Configuration into AppConfig class, then creating the Desktop object under the class. Then add annotation @Bean

## give name to the bean
the method name is the bean name, u can give multiple name after @bean

## scope annotation - how to use prototype for desktop
@Scope("prototype)
- this will create multiple objects
- only when when u create it, not when loading the config file

## autowire
Add @bean for Alien class

@Autowire Computer which is a super Interface

## primary bean
if u autowire Computer com, and have 2 types of Computer bean. Spring cannot find which is the right Computer bean

u can use the @Qualifier("Desktop) to specify this to be used

or we can use @primary to make the bean a primary bean

## Component stereotype annotation
Alien and Desktop class does not know if they are managed by spring bean

Add @Component on top of all the classes, the adv is u do not need to write xml or java-based config

Add @ComponentScan("com.package); 

## autowire computer interface
Yes, auto look for any beans that has the Computer interface
then wire Desktop into Alien obj

@Autowire
@Qualifier("laptop") //bean name is the class name, just small cap

you can also specify the name of the name by @Component("com2"), so Desktop obj will have a name of com2

3 levels of injection
1. field injection
- @Autowired private Computer com;
2. constructor injection
3. setter injection
best to use above setter

## qualifier vs. primary
qualifier precede primary, qualifier will be given precedence in instruction.

## scope and value annotation
scope - singleton and prototype

how to inject a value -> @value("21); when u use annotation, you are injecting from outside the code

```
//org.example/config/AppConfig
package org.example.config;

import org.example.Alien;
import org.example.Desktop;
import org.example.Computer;
import org.example.Laptop;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.context.annotation.*;

//tell spring this is a java-based config file to manage beans
@Configuration
//scan this component, and create bean when necessary, then we do not need to specify each bean one by one
@ComponentScan("org.example")
public class AppConfig {
//    @Bean
//    //autowire link Alien to Desktop
//    //when u autowire to Computer interface, it will look for desktop bean
//    //however, when u have 2 of the same Computer type, need to set primary Bean
//    public Alien alien(@Autowired Computer com){ // @Qualifier("desktop")
//        Alien obj = new Alien();
//        obj.setAge(25);
//        obj.setCom(com);
//        return obj;
//    };
//
//    //return a Desktop obj
//    //must tell spring this is a bean
//    //u can give multiple name in {} to the bean
////    @Bean(name= {"com1", "com2"})
//    @Bean
//    @Primary
//    //method name "desktop" is the default bean name
//    //Scope prototype allows u to create multiple obj
////    @Scope("prototype")
//    public Desktop desktop(){
//        return new Desktop();
//    }
//
//    @Bean
//    public Laptop laptop(){
//        return new Laptop();
//    }
}

package org.example;

import org.springframework.context.annotation.Primary;
import org.springframework.stereotype.Component;


//u can give a name to the bean
@Component("desktop2")
//@Primary
public class Desktop implements Computer {
    public Desktop() {
        System.out.println("desktop obj created");
    }

    public void compile(){
        System.out.println("compiling in desktop");
    }
}

package org.example;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Scope;
import org.springframework.stereotype.Component;


@Component
//@Scope("prototype")
public class Laptop implements Computer {
    public Laptop(){
        System.out.println("laptop obj created");
    }
    @Override
    public void compile(){
        System.out.println("compiling in laptop");
    }
}

package org.example;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.stereotype.Component;

import java.beans.ConstructorProperties;
@Component
public class Alien {
    //auto find any Computer beans and inject here
//    @Qualifier("desktop")
    private Computer com;


    private int age;

//    @ConstructorProperties({"laptop","age"})
//    public Alien(Laptop laptop, int age) {
//        System.out.println("multi para constructor called");
//        this.laptop = laptop;
//        this.age = age;
//    }

    public Alien(int age) {
//        System.out.println("age constructor called");
        this.age = age;
    }

    public Computer getCom() {
        return com;
    }

    @Autowired
    @Qualifier("desktop2")
    public void setCom(Computer com) {
        this.com = com;
    }

    public int getAge() {
        return age;
    }

    @Value("25")
    public void setAge(int age) {
        this.age = age;
    }

    public Alien(){
        System.out.println("Alien obj created");
    }
    public void code(){
        System.out.println("coding");
        com.compile();
    };

}

package org.example;

import org.example.config.AppConfig;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

/**
 * Hello world!
 */
public class App {
    public static void main(String[] args) {
        ApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);
        Alien al = context.getBean(Alien.class);
        al.code();
        System.out.println(al.getAge());
    }
}

```

