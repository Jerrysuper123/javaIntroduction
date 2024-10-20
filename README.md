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

package is a folder in Java class to avoid naming conflict of class, a way to group related classes under the package umbrella

### Java folder structure
`Com.package2.Main`

- Com is a folder
- Package2 is a subfolder
- Main is the class

Although intellij at SRC level collapse the 2 folder into 1 "com.package2"; this is actually nested folder; you can see the out folder layout, which is the byte class code

![folder](./folder.jpg)

So that  you could type Java in Main class for execution.

## How Java is excecuted?
Source code.jva => compiler => byte code.class

You can do this compilation in terminal as well:
- Java byte code is platform independant
- Java byte code => JVM java virtual machine => Linux, windows etc.
  - C++ and python has the same way of working
  - that is why it is platform independant
 
In intellij, it also has the same folder structure:
- SRC code folder
- Out code folder - for byte code class file

## Types of Java edition
Standard edition (SE)
- Enterprize edition is built on top of the SE and made to build scaleable enterpize software
  
Micro edition
- Used to build for mobile device

Card edition 
- Used to build for card device

## Convention: every Java program must have the same name as java class
Main.java to class Main
- Every program also must have a main method, might be named differently
  - This main class will wrap every other class under it 
- Because it is execute first

## U can println(num) directly without quotation mark
```
system.out.println(3);
```

# Main Java programming concept

## Java primitive variables types
There are a lot more, below is just the basics
Float, long, double must be ended with f, l, and d
- It seems only float number needs to be ended with f, with higher version of Java
```
        //1 byte 2^8=> 256 combination signed number between -128 and 127
        byte number = 125;

        //2 bytes 2^16
        short numberShort = -23;

        //4 bytes
        int numberInt = 34;

        //8 bytes
        long numbgerlong = 1234;

        //4 byte store fractional number, 6-7 decimal place
        float numberfloat = 0.3f;

        //8 byte, store 15 decimal digits
        double numberDouble = 0.4;

        //1 bit
        boolean married = true;

        //2 byte usually for ascii charadter
        char alphabet = 'B';
```
## the above is primitiave, while non-primitive is upper-cased and come with defined functions in Java
Examples of non-primitive types are Strings, Arrays, Classes, Interface, etc. 

Primitive is defined by Java, but non-primitive is defined by programmer. Non-primitive can be used to call methods to perform certain operation, while primitive cannot.
- String seems to be non-primitive
- So it should have some built in functions

## final variable, similar to const in JS, where you cannot update the value once assigned
```
final int text = 32;
//cannot assign a new value to final var
text = 22;
```

## Type casting - changing varable from 1 type to another; widening is automatic while narrowing needs manual
In Java, there are two types of casting:
Widening Casting (automatically) - converting a smaller type to a larger type size
- byte -> short -> char -> int -> long -> float -> double

Narrowing Casting (manually) - converting a larger type to a smaller size type
- double -> float -> long -> int -> char -> short -> byte

Automatic widening
```
public class Main {
    public static void main(String[] args) {
       int intNum = 9;
       double myDouble = intNum; //automatic casting from int to double

        System.out.println(intNum); //9
        System.out.println(myDouble); //9.0
    }
}
```

Manual narrowing
```
public class Main {
    public static void main(String[] args) {
       double myDouble = 9.78;
       int intNumber = (int) myDouble; //narrowing type needs manual intervention

        System.out.println(myDouble); //9.78
        System.out.println(intNumber); //9
    }
}
```

```
public class Main {
    public static void main(String[] args) {
       int number = (int) (Math.random()*101); //Math.random() will be double, so neeed to manually cast to int
    }
}
```

## Just like JS, Java has ternary operator

```
public class Main {
    public static void main(String[] args) {
       int time = 21;
       String result = time>20? "more than 20": "less than 20";
        System.out.println(result); //more than 20
    }
}
```

## Java array is in curly bracket
```
public class Main {
    public static void main(String[] args) {
        //Unlike JS, java array in curly bracket
       String[] cars = {"Volvo","BMW"};
       for(String i: cars){
           System.out.println(i);
       }
    }
}
```

## Static - method belongs to the main class and not an instance of the main class; void means the method does not have a return value
```
public class Main {
    //a method that returns nothing and belongs to the class itself
    static void myMethod() {
        System.out.println("Executing...");
    }

    //main method
    public static void main(String[] args) {
        myMethod();
    }
}
```

## Java method overloading - same method but for different parameters - rather than defining different functionName doing the same thing,why not use the same func name for readibility and flexibiity

In summary, method overloading helps keep your code clean, organized, and easier to maintain, while also improving readability and flexibility.

Still not seeing the benefits

```
public class Main {
    //two different method names but doing the same function
    static int plusMethodInt (int x, int y){
        return x + y;
    }

    static double plusMethodDouble (double x, double y){
        return x + y;
    }
    
    //method overloading - same method name but different parameters
    static int plusMethod (int x, int y){
        return x +y;
    }

    static double plusMethod (double x, double y){
        return x +y;
    }
    
    //main method
    public static void main(String[] args) {
        System.out.println(plusMethod(3,2));
        System.out.println(plusMethod(3.1,2.3));
    }
}
```
# OOP - Object oriented programming
Put data and methods in a class for easy manipulation

## Attribute - data within a class
When creating a new instance => <typeClassName> instance = new className()

```
public class Main {
    int x = 5;

    //main method
    public static void main(String[] args) {
        //create an instance within the class itself
        Main instanceMain = new Main();
        System.out.println(instanceMain.x); //output 5
    }
}

```

You can override the existing attribute of the class
```
public class Main {
    int x = 5;

    //main method
    public static void main(String[] args) {
        Main instanceMain = new Main();
        instanceMain.x = 10;
        System.out.println(instanceMain.x); //output 10
    }
}
```

## static method can be called without creating an instance, public method must be called by creating an instance, public generally means can be accessed from the outside
```
public class Main {
    static void myStaticMethod(){
        System.out.println("Static method can be called without creating an object");
    }

    public void myPublicMethod(){
        System.out.println("public method must be called with an object");
    }

    //main method
    public static void main(String[] args) {
        myStaticMethod(); //staic  method can be called right away
//        myPublicMethod() => there is an error

        Main mainObject = new Main();
        mainObject.myPublicMethod(); //this can be called with an object
    }
}
```

## constructor is used to construct the new instance of the class, can be used to set initial values of the obj; construct method name is the same name as the class name

And constructor does not need returnType
```
public class Main {
    int modelYear;
    String modelname;

    //Contructor has the same method name as the Main class
    public Main(int year, String name){
        modelname = name;
        modelYear= year;
    }

    //main method
    public static void main(String[] args) {
        //Create a new instance of the class, this will call Main constructor
       Main newCar = new Main(1937, "BMW");
        System.out.println(newCar.modelname + " " + newCar.modelYear);
    }
}
```

## Main method - public static void main(String[] args){}
public static void main(String[] args)
- why public (JVM access from outside), JVM needs to find the Main method to invoke it
- then static (allow no object creation to access main funct), otherwise JVM cannot access it, unless creating an object
- String[] args  (allow command line java hello arg 1 arg2), this allows use to add some arguments from the commandline


Make a java file
```
// HelloWorld.java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
           if (args.length > 0) {
            System.out.println("there is args");
        } else {
            System.out.println("there is no args");
        }

    }
}
```


Run in the terminal
```
user@host % java HelloWorld 
Hello, World!
user@host % vim HelloWorld.java
user@host % javac HelloWorld.java 
user@host % java HelloWorld 
Hello, World!
there is no args
user@host% java HelloWorld "hehe"
Hello, World!
there is args
```

## How to compile and run from the terminal
create a java file with .java extension. for example, HelloWorld.java
```
// HelloWorld.java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}

```

Compile java program
```
javac HelloWorld.java
```

This will generate a HelloWorld.class file. Take note we run the program without .class extension
```
java HelloWorld
```

## Accesss modifier and non access modifier

Acess modifier - control the access to classes, methods or variable
- public - accessed by any classes
- private - only accessed within its own class
- Protected - accessed within its own class, subclasses, and other classes within the package
- default (no need to write this), accessed within its own class, subclasses and other classes within the same package

I am a person:
- my age is public information
- my fetish is private to me only, and my son does not know it
- my wealth is protected
  - private to other people
  - but my son (subclass) knows about it
 
  No access modifier - provide additional info about the class, and methods
  - static - element belong to the class rather than instance
  - final - similar to JS const
  - Abstract - the element is incomplete and must be overriden by subclasses, method body is not completed
 
  There are a lot more other modifier...

## Abstract class - provide template for its subclass, but does not provide method body

```
//Abstract define a template
abstract class Student {
    public String name = "john";
    public int age = 32;
    public abstract void study(); //no method body
}

//Subclass can inherit the attribute and method name, fill in method body
public class John extends Student{
    public int graduationyear = 2023;
    public void study(){
        System.out.println("studying now");
    }
}

public class Main {
    //main method
    public static void main(String[] args) {
        John john = new John();
        System.out.println(john.name); //John
        System.out.println(john.age); //32
        System.out.println(john.graduationyear);
        john.study(); //studying now
    }
}
```

## Private - encapsulation - hide sensitive data from user, only getter and setter can access it
```
public class Person {
    public String name;
    private int bankAccount;

    public Person(String nameOnIC, int amount){
        name = nameOnIC;
        bankAccount = amount;
    }
    //getter
    public int getBankAccount() {
        return bankAccount;
    }

    //setter
    public void setBankAccount(int bankAccount) {
        this.bankAccount = bankAccount;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}

public class Main {
    //main method
    public static void main(String[] args) {
        Person John = new Person("john", 234);
        System.out.println(John.name);
        System.out.println(John.getName());
       // System.out.println(John.bankAccount); // error cannot access it directly
        System.out.println(John.getBankAccount());
    }
}
```

In banking, we can keep balance private, using getter and setter to control what value to update
```
public class BankAccount {
    private double balance;

    public BankAccount(double amount){
        balance = amount;
    }

    public double getBalance() {
        return balance;
    }

    //setter modified
    public void deposit(double amount) {
        if(amount>0){
        this.balance += amount;
        }
    }

    public void withdraw(double amount){
        //why i can access balance here directly
        if(amount >0 && amount <=balance){
            this.balance-=amount;
        }
    }
}

public class Main {
    //main method
    public static void main(String[] args) {
        BankAccount bankAccount = new BankAccount(300);
        System.out.println(bankAccount.getBalance());
        //System.out.println(bankAccount.balance); error to access private balance directly
        bankAccount.deposit(30);
        bankAccount.withdraw(20);
        System.out.println(bankAccount.getBalance()); //310
    }
}
```
## Checked vs unchecked exceptions
Checked exceptions must be handled at compile time (either by catching or declaring them), whereas unchecked exceptions can be ignored by the compiler and handled (or not) at runtime.

## inheritance - inherit methods and attributes from another class

subclass/child class - the class that inherit

super/parent class - the class being inherited from

### you can use final if you do not want the other class to inherit it
```
final public class Vehicle {
    int model = 1998;
}

public class Car extends Vehicle{
    //cannot inherit from final class
}

```

## polymorphism - subclasses are related by inheritance and perform differently (siblings have many formts) - same method but performs differently
```
public class Animal {
    public void animalSound(){
        System.out.println("this animal makes a sound");
    }
}

public class Cat extends Animal {
    public void animalSound(){
        System.out.println("Cat meow meow");
    }
}

public class Dog extends Animal {
    public void animalSound(){
        System.out.println("dog sound");
    }
}

public class Main {
    //main method
    public static void main(String[] args) {
        //Dog and Cat inherit from the same parent, but behave differently
        Dog dog = new Dog();
        dog.animalSound(); //dog sound

        Cat cat = new Cat();
        cat.animalSound(); //Cat meow meow
    }
}
```
## Java inner class - nested class in another class
Below might not work in Java 17
```
class OuterClass {
  int x = 10;

  private class InnerClass {
    int y = 5;
  }
}

public class Main {
  public static void main(String[] args) {
    OuterClass myOuter = new OuterClass();
    OuterClass.InnerClass myInner = myOuter.new InnerClass();
    System.out.println(myInner.y + myOuter.x);
  }
}
```

## Interface (sound like ts interface, but not) is complete abstract class for methods but with no body. Has to use implement words to inherit properties. also, you can inherit multiple interface, while you can only inherit 1 class
@Override annotation is used to indicate that a method is overriding a method from its superclass or implementing an interface method. It helps to ensure that a method is being correctly overridden, and it improves the clarity and maintainability of the code.

```
public interface FirstInterface {
    public void myMethod();
}

public interface SecondInterface {
    public void myOtherMethod();
}

public class DemoClass implements FirstInterface,SecondInterface{
    @Override
    public void myMethod() {
        System.out.println("myMethod");
    }

    @Override
    public void myOtherMethod() {
        System.out.println("my other method");
    }
}

public class Main {
    //main method
    public static void main(String[] args) {
        DemoClass demoClass = new DemoClass();
        demoClass.myMethod();
        demoClass.myOtherMethod();
    }
}
```
## enum is a group of final constants, special class, structured group and type safe
```
public enum Level {
    LOW,
    MEDIUM,
    HIGH
}

//This line tells me which package i am at now for this class
package mycompany.learningpackage;

public class Main {
    //main method
    public static void main(String[] args) {
        //I can use enum without importing them??
        Level myVar = Level.MEDIUM;
        System.out.println(myVar);
    }
}

```

### scanner used to receiver user input
```
import java.util.Scanner;

public class Main {
    //main method
    public static void main(String[] args) {
        Scanner myObject = new Scanner(System.in);
        System.out.println("Enter name, age and salary");
        String name = myObject.nextLine();
        int age = myObject.nextInt();
        double salary = myObject.nextDouble();
        System.out.println("Name" + name);
        System.out.println("age:" + age);
        System.out.println("salary" + salary);
    }
}
```

### How to format date
```
import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;

public class Main {
    //main method
    public static void main(String[] args) {
        //.now to get, not new class
        LocalDateTime myDateObj = LocalDateTime.now();
        System.out.println("Before formatting..." + myDateObj); //Before formatting...2024-10-19T10:54:25.019833

        //you can format date using below
        DateTimeFormatter myFormatter = DateTimeFormatter.ofPattern("dd-mm-yyy HH:mm:ss");

        String formattedDate = myDateObj.format(myFormatter);
        System.out.println("after formatted.." + formattedDate); //after formatted..19-56-2024 10:56:03
    }
}
```

### ArrayList is a resizable length of array
```
import java.util.ArrayList;
import java.util.Collections;

public class Main {
    //main method
    public static void main(String[] args) {
        ArrayList<String> cars = new ArrayList<String>();
        cars.add("Volvo");
        cars.add("BMW");
        cars.add("Mazda");

        System.out.println(cars);//[Volvo, BMW, Mazda]

        //loop by counter
        for(int i=0; i<cars.size(); i++){
            System.out.println(cars.get(i));
        } //Volvo BMW Mazda

        //quick loop
        for (String car: cars){
            System.out.println(car); //Volvo BMW Mazda
        }

        //sort
        Collections.sort(cars);
        System.out.println(cars); //[BMW, Mazda, Volvo]

        //access the element
        String firstElement = cars.get(0);
        System.out.println(firstElement); //BMW

        //set new element
        cars.set(0, "Opel");
        System.out.println(cars.get(0)); //OPel

        cars.remove(0);
        System.out.println(cars.get(0)); //Mazda

        cars.clear();
        System.out.println(cars);
    }
}
```
## Integer is a wrapper class for int with more utility function, when initializing arrayList, we must use Integer instead of int, because Collections only accept object instead of primitive
```
public class Main {
    //main method
    public static void main(String[] args) {
        //Must use wrapper class for collections - Integer below instead of int
        ArrayList<Integer> cars = new ArrayList<>();
        
    }
}
```
int is a primitive data set, while Integer is a class that wraps an int value in an object.

## Autoboxing - convert primitive to object, unboxing - convert object to primitive

```
public class Main {
    //main method
    public static void main(String[] args) {
        int primitiveInt = 42;
        Integer integerObject = new Integer(42);//explicit creating an object

        Integer autoboxed = primitiveInt; //autobox - auto convert primitive to an Integer

        int unboxed = integerObject; //unbox - integer to int

        System.out.println(autoboxed); //42
        System.out.println(unboxed);//42
    }
}
```

## Linkedlist - similar to lInkedlist data structure
LinkedList has the same methods as arraylist because both implements the same List interface.
```
public class Main {
    public static void main(String[] args) {
        LinkedList<String> ls = new LinkedList<>();
        ls.add("bmw");
        ls.add("asta");

        ls.addFirst("haha");

        System.out.println(ls); //[haha, bmw, asta]
    }
}
```

## Hashmap - create key value pair object
```

public class Main {
    public static void main(String[] args) {
        HashMap<String, String> capitals = new HashMap<>();
        capitals.put("China", "Beijing");
        capitals.put("US", "Washington");

        System.out.println(capitals); //{China=Beijing, US=Washington}

        System.out.println(capitals.get("China")); //Beijing
    }
}
```

## Iterator loops though collections
```
// Import the ArrayList class and the Iterator class
import java.util.ArrayList;
import java.util.Iterator;


public class Main {
  public static void main(String[] args) {


    // Make a collection
    ArrayList<String> cars = new ArrayList<String>();
    cars.add("Volvo");
    cars.add("BMW");
    cars.add("Ford");
    cars.add("Mazda");


    // Get the iterator
    Iterator<String> it = cars.iterator();


    // Print the first item
    System.out.println(it.next());
	while(it.hasNext()) {
  System.out.println(it.next());
}


  }
}
```

## super - calling constructor of parent class
```
public class Animal {
    String animal;

    public Animal(String animal){
        this.animal = animal;
    }
    public void animalSound(){
        System.out.println("this animal makes a sound");
    }
}

public class Cat extends Animal {
    //Cat constructor
    public Cat(String animal) {
        //calling constructor of the parent class Animal
        super(animal);
    };
    public void animalSound(){
        System.out.println("Cat meow meow");
    }
}
```

## Java exceptions 
```
public class Main {
    public static void main(String[] args) {
        try{
        int[] numbers = {1,2,3};
        System.out.println(numbers[10]); //java.lang.ArrayIndexOutOfBoundsException: Index 10 out of bounds for length 
        } catch (Exception e){
            System.out.println(e);
        } finally {
            System.out.println("try and catch is finished");
        }
    }
}
```

Here are a few examples of predefined exceptions:
- ArithmeticException: Thrown when an arithmetic operation cannot be performed correctly, such as division by zero.
- NullPointerException: Thrown when an attempt is made to access a member (method or field) on a null object reference.
- ArrayIndexOutOfBoundsException: Thrown when attempting to access an array element at an invalid index.
- IOException: Base class for exceptions related to input and output operations.
- ClassNotFoundException: Thrown when a class is not found by the classloader.
- NumberFormatException: Thrown when an invalid string representation is parsed to a number.

## define customer exception, whenever you want to make use of it in a class - it must start with a throws word

```
public class InsufficientFundsException extends Exception {
    public InsufficientFundsException(String message){
        //The super keyword refers to the parent class (in this case, Exception),
        // and this ensures that the custom exception message is properly handled by the base exception class.
        //One of the most common uses of super is to call the constructor of the parent class.
        super(message);
    }
}
```

```
public class BankAccount {
    ...

    public void withdraw(double amount) throws InsufficientFundsException{
        //why i can access balance here directly
        if(amount>balance){
            throw new InsufficientFundsException("Insufficient bank balance");
        }
        if(amount >0 && amount <=balance){
            this.balance-=amount;
        }
    }
}

```

```
public class Main {
    public static void main(String[] args) throws InsufficientFundsException {
        BankAccount ba = new BankAccount(30.3);
        ba.withdraw(31); //Insufficient bank balance
    }
}
```
