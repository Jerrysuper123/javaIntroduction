# javaIntroduction

## dependency injection
It is for writing better code and testing
- We do not instantiate a class within the parent class
- Each class is separate and isolated
- So that every class can be mocked

Dependency injection
- inject into the parent class by passing external dependency into the constructor of the parent class
- However, it is troublesome to generate all new instances everywhere, filled to the brim of the heap
- That is why we use singleton - one instance that could be used anywhere e.g. @bean in springboot

```
@Component
public class MOTService{
  private EmailService emailService

//@inject = EmailService emailService = new EmailService()
  @Inject
  public MOTService(EmailService emailService){
    this.emailService = emailService:
  }
}
```

## Java is close to C++ and C#, making it easier to switch to Java or vice versa
Class is just a group of related methods and variable/object
- Class Naming is pascal case
- Method naming is camel case

General structure
```
class Main {
  public void main {
    acccessModifier ReturnType methodName(parameterType parameter){
    }
  }
}
```

## static
Method that belongs to the class.
You cannot get it from the instance of the class




## Com.package is to group related classes
If we have 100 to 1000 classes, it is better to group and organize in package/categories.
So Com.package is a folder under the root folder.

### Java folder structure
`Com.package.Main`

- Com is a folder
- Package is a subfolder
- Main is the class

(folder)[./folder.jpg]

So that  you could type Java in Main class for execution.

## How Java is excecuted?
