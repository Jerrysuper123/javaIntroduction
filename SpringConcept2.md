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
