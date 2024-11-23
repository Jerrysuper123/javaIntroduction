# JDBC connection - get query
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
