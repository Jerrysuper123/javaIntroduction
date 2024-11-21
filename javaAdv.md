
# Jshell allows you to interact just like python terminal
```
jshell - java 9 - allow u to add 3+2, this will output some value
|  Welcome to JShell -- Version 17.0.12
|  For an introduction type: /help intro
jshell> 2+3
 $1 ==> 5
jshell> System.out.print("hello");
hello
```

# JVM can only understand bytecode
you create Java code (english) -> Java compiler -> bytecode run by JVM

JVM needs a first file to find and run, that first file needs to have a main method, where execution starts
- Public static void main(Strings[] args){}

# what is String a[]
```
// String a[]: This syntax places the array brackets ([]) after the variable name, which is an older Java syntax style.
// String[] args: This places the array brackets next to the type (String), which is the recommended and more modern syntax according to Java's conventions.
```

# what is JRE
JVM and Lib are part of JRE.
JRE helps to run JVM and manage Jib. JRE is installed on OS.
JDK17 contains JRE.

# what is the diff betwen print and println
println prints a new line, while print does not.

# primitive 
interger type - int, long, short, byte
float - double and float
char - 2byte (support unicode)
boolean

# literal is string or char in single or double quote

```
        int num1 = 0b101; //print binary and hex values on screnee

        //underscore just a way for programmer to see clearly
        int num2 = 100_100_1000; //1001001000
        double num3 = 12e12; //1.2 ek^12
        System.out.println(num3);
```

# auto widening and manual narrowing (due to some data might lose)

implicit vs explicit conversion

data lose due to narrowing
```
        float f = 5.6f;
        int x = (int) f;
        System.out.println(x); //5 - lose 0.6
```
# short cut for compile and run java
below only work for small file

```
java Hello.java
```

there is type promotion - when 2 byte varible times each other, result in 300, this has to be converted into a integer, cos out of range.

# what is literals in java
In Java, literals are fixed values directly written in the code that represent constant values. They are assigned to variables as part of the code and do not change. Java supports several types of literals:

it can be integer, float, character, string or anything.

so long literal is a int 3003L;

# pre and post increment
post-increment operator, which means var is evaluated first, then incremented.
```
    int num = 7;
        int result = num++; //post increment - first fetch (means result = num) value then increase (num+1=8)
        System.out.println(result); //7
        System.out.println(num); //8

                
        int num2 = 7;
        int result2 = ++num2; //pre increment - first increase (num2+1 = 8) then fetch value (result2=8)
       System.out.println(result2); //8
```

```
x=5
y=10
Final Values of x and y

    x was incremented to 6 due to x++.
    y remains 10 because y-- was never evaluated.

int z = (x++ > 5 && y-- < 10) ? x-- : y;

Step-by-Step Analysis

    Evaluate x++ > 5:
        The expression x++ uses the post-increment operator, which means x is evaluated first, then incremented.
        Initially, x is 5, so x++ > 5 is equivalent to 5 > 5, which is false.
        After this operation, x is incremented to 6.

    Evaluate y-- < 10:
        Since the first condition (x++ > 5) is false, the second condition (y-- < 10) is not evaluated due to short-circuiting in the && operator.
        This means y remains 10; it is not decremented.

    Conditional (Ternary) Operator ?::
        Since the condition (x++ > 5 && y-- < 10) is false, the else part of the ternary operator is executed, which assigns y to z.
        So, z = y, which means z = 10.

```

# Loop - has 3 things, initial value, increment and condition
```
//while sth is true do sth, while it is false, do not do
//do sth first, after that while it is true do sth, stop when it is false
//for loop - initial value, condition and increment
//for advanced int n: array
//forEach

         int i = 5;
        do { 
            System.out.println("Hi" + i);
            i++;
        } while (i<=4);

        for(int j=0; j<=4; j++){
            System.out.println(j);
        }

Read file and db, usually use while loop
for loop is for print 1-100
```

# JVM creates an object from the class /blue print
We design class but JVM create an object
Design in Californa and made in China
an object created out of class is called reference object.

# JDK JRE JVM
JVM to run your app, JRE to load dependencies for your app during runtime. And JDK encapsulates both JVM and JRE.

Java development kit - Top layout JDK encapsulates JVM and JRE
Java virtual machine - a virtual space on top of the OS to run your code. JVM is part of JRE
Java runtime environment - load extra class/dependencies into JVM.  

# method overloading makes it flexible to use same method to take in different parameters and return diff results
Method can take in 2 or 3 parameters with the same method name;

# JVM memory - stack and heap
Stack - last in first time (every method has its own stack)
heap - contains object (method and properties) created by class; this object also has its address e.g. 101. Then obj=101 will be stored in the stack

source: https://www.udemy.com/course/spring-5-with-spring-boot-2/learn/lecture/35533208#overview

# array is continuous array - fixed
only 3 boxes, then 3 boxes, cannot change
int is fixed for each box

```
      int num1[] = new int[3];
```
so collection is better than array

# by default, integer array element is 0, unless provided otherwise.

Exception - runtime error

# String is capitalized compared to primitive, so it is a class

```
class Student{
    //create new obj string in heap with address 102
    //stack newName will point to add 102
    String newName = new String("New name"); //=String newName = "New name"
}

class Hello{
    public static void main(String a[])
    {
        Student s1 = new Student();
        System.out.println(s1.newName); //New name
        System.out.println(s1.newName.charAt(1)); //e
        System.out.println(s1.newName.hashCode()); //some hashcode
     
    }    
}
```

# string constant pool - once u create the string object, you cannot change it, new var is just pointing to new address

by default, String is immutable String.
For mutable string, you can use StringBuffer and StringBuilder

```
class Hello{
    public static void main(String a[])
    {
        //JVM contains both stack (local variables) and heap (contains object created)
       String name = "Tom"; // name points to address 102 = "Tom"; Tom is not mutated, still in heap
       name = name + " Cruise"; //name points to address 103 = "Tom Cruise"
       System.out.println(name); //Tom Cruise
    

       //String constant pool to save memory
       //S1 and S2 are pointing at the same address to "Navin"
       String S1 = "Navin";
       String S2 = "Navin";

       System.out.println(S1==S2); //true 
     
    }    
}
```


# mutable string - string buffer is thread safe, while string builder is not thread safe
String buffer is thread safe, sync threads to allow only 1 method to access the string to prevent race condition.

```
class Hello{
    public static void main(String a[])
    {
        //StringBuffer default capacity is 16, but it expand size depending on your string size
         StringBuffer sb = new StringBuffer();
        System.out.println(sb.capacity()); //default 16 size
        StringBuffer sb2 = new StringBuffer("Navin"); //expand capacity
        sb2.append(" Tom");
        System.out.println(sb2);

        // you need to use toString to convert to string before using
        String str = sb2.toString();
        System.out.println(str);

        //string buffer is thread safe, while string builder is not thread safe
    }    
}

```

# local variable - variable inside a method, static variable - a common var to all objects created from the class. commonly accessed by the class name

```
class Student{
    //instance variable
    String name;
    //static variable - accessed through class
    static String className;
    
}

class Hello{
    public static void main(String a[])
    {
        Student std1 = new Student();
        std1.name = "Tom";

        Student std2 = new Student();
        std2.name = "Jelly";

        std1.className= "4A1";    

        System.out.println(std1.className); //4A1
        System.out.println(std2.className); //4A1

        System.out.println(Student.className); //4A1
    }    
}

```

# static method is non instance method
You cannot use non static variable in static method.
Static main - that is why it is static, jvm to call main without creating the object first

# static block - will only be called once
static block is called before constructor

First load class then create instances
That is why static block in class is created first, then constructor is called to create objects.

```


class Student{
    String name;
    static String className;

    public Student(){
        name = "tom";
        System.out.println("in constructor"); //will be called twice
    }

    //static block only loads once
    static {
        className = "4M1";
        System.out.println("in static block"); //called once only
    }
    
}

class Hello{
    public static void main(String a[]) throws ClassNotFoundException
    {
        //if no object is created, static block will not be called
        // Student st = new Student();
        // Student st2 = new Student();


        //Below will load class, but will not create an object
        Class.forName("Student");
    }    
}

```

# encapsulation - keep everything outside, so no one can use it

Private - it is only accessible within the class, not from outside (instance)
They should able to access it, but only through method within the same class; that is why we are using getter and setter; basically we are binding the data in methods


# this in class refers to the current object/instance

# if you do not have constructor, you will create an empty object; constructor helps to create an object with some values initially

Everytime you create an object, you are calling constructor method by default, even if you have not written anything

# default vs. parameterized constructor 

```
class Student{

    String name;
    int className;
   
    //default constructor with no parameter
    //this will generate empty object = {name: null, className: 0}
    public Student() {
    }

    //parameterized constructor
    public Student(String name, int className) {
        this.name = name;
        this.className = className;
    }

    
}

class Hello{
    public static void main(String a[]) throws ClassNotFoundException
    {
     Student st1 = new Student();
     Student st2 = new Student("tom", 33);

    System.out.println(st1.name + st1.className); //null 0
    System.out.println(st2.name + st2.className); //tom33
    }    
}
```

# anonymous object - create an object with assigning it to the variable. so it is created in the heap, but not assigned in the stack

```
New student();
```

# java inheritance helps to reduce redundancy - just extends to get all the features from another class

Subclass extends SuperClass

# multi-level extension
AdvCal (* /) extends Cal (=-)
VeryAdvCal (power) extends AdvCal to get * /, + -.


# multiple inheriance - a child class to inherit from 2 parent class
multiple inheritance will not work in Java

C class is calling y() method, but parentA class has y() method and parentB class has y() method, which one to choose. 

The above causes ambiguity, so Java removes this to resolve the issue.

# when calling constructor, it will call super, then sub class constructor

by default, every constructor will have a super();

super means call the constructor of the super class first.

```

class A extends Object{
    public A(){
        super(); //calling super class Object
        System.out.println("A");
    }

    public A(int num){
        System.out.println("A with int");  
        }
}

class B extends A{
 public B(){
    //call super class constructor
    this(2);
    // super(3); 

    System.out.println("B");  
    }

    public B(int num){
        super();
        System.out.println("B with int");  
    }
}


class Hello{
    public static void main(String a[]) throws ClassNotFoundException
    {
        B obj = new B(); //A B

    }    
}
```

# every class in Java extends to Object class, even if you do not mention.

```
Class A extends Object
```

# method override - same method name, sub method override the super method

method overloading is being overloaded with diff parameters for the same method name.

method overriding to replace the super class method implementation with new logics in sub class, for the same method name

# package is a folder

Import all files from folder java/lang
By default we do not have to write below
* means all the file, not all the folders

```
import java.lang.*
```

How to make package name unique? we use domain name but reverse it.

```
import com.google.cal
```

```
// import entity.Kid;
// import entity.Student;
import entity.*; //* stands for all files in the folder */

class Hello{
    public static void main(String a[]) throws ClassNotFoundException
    {
        
        Student st = new Student();
        Kid kid = new Kid();
    }    
}

```

# access modifier
If in the same package, sub class could accces super class - int marks;

But if not in the same package, we need to public int marks, for sub class to access super class;

public can be accessed from anywhere.

# you always mark your variable private.make your method public

there is default, you can only acccess from the same package
```
int mark
```

- make var private
- make method public
- if you only want sub class to access, make it protected.

# poly - morphism (many behaviours)
Behave every differently in different situations

Different polymorphism (compile vs runtime)
- Compile time (overloading method - same method name, different behaviour)
- run time (overriding method - same method name, but sub overriding super)


# dynamic method dispatch
We are not sure which method will be called through overriding method.

It will be only dynamically called at runtime.

# final can be used for var, method and class
once u make a class final, no one can extends it.

Make method final, no one can override the final method

# every class naturally extends to Object class. every time you call obj, you can actually calling obj.toString() method from object class

What is the use of toString() method? it returns class name + @ and hexString

What is hashcode? generate hash value based on the data you have.

```
class B {
    public B(){
        System.out.println("in B");
    }
}

public class Demo {
        public static void main(String[] args) {
            B obj = new B();
            //toString() is a method from Object class
            System.out.println(obj.toString()); //B@7344699f

            //Below is the toString in Object class
            //that is why we need to override this, when we want to print out the obj in the console for debuggin
            // public String toString() {
            //     return getClass().getName() + "@" + Integer.toHexString(hashCode());
            // }
        }
}
```

# override the toString() method to see underlying object
```
class B {

    int num = 3;
    String model = "model3";

    public B(){
        System.out.println("in B");
    }

    public String toString(){
        return num + " : " + model;
    }

}

public class Demo {
        public static void main(String[] args) {
            B obj = new B();
            //remove .toString() from below, since .toString is automatic
            System.out.println(obj); //3 : model3
        }
}


```

# toEqual from Object class compares the hexdecimal hash of 2 values. We want to compare the value only, so we set up equals ourself

Hashcode() produce uniqiue integer tied to each instance in heap of Jvm.
Equals () actually compare the unique integer of 2 objects.

IDE has auto generating equal and hashcode functions

by default equals() compares the unique integer rep that instance in heap memory
```
class B {

    int num;
    String model;

}

public class Demo {
        public static void main(String[] args) {
            B obj = new B();
            obj.num = 3;
            obj.model = "model3";

            B obj1 = new B();
            obj1.num = 3;
            obj1.model = "model3";

            System.out.println(obj==obj1); //false
            System.out.println(obj.equals(obj1)); //false
        }
}
```

Below implements custome equals();
```
class B {

    int num;
    String model;

    public boolean equals(B that){
        return this.model == that.model && this.num == that.num;
    }

}

public class Demo {
        public static void main(String[] args) {
            B obj = new B();
            obj.num = 3;
            obj.model = "model3";

            B obj1 = new B();
            obj1.num = 3;
            obj1.model = "model3";

            System.out.println(obj.equals(obj1)); //true
        }
}

```

Auto generate toString, equals and hashcode()
- toString does not print out obj
- equals only compares the unique integer rep each instance, it does not mean the underlying attribute values in ab object

```
class B {

    int num;
    String model;
    

    //auto generating toString

    @Override
    public String toString() {
        return "B [num=" + num + ", model=" + model + "]";
    }

    @Override
    public int hashCode() {
        final int prime = 31;
        int result = 1;
        result = prime * result + num;
        result = prime * result + ((model == null) ? 0 : model.hashCode());
        return result;
    }
    @Override
    public boolean equals(Object obj) {
        if (this == obj)
            return true;
        if (obj == null)
            return false;
        if (getClass() != obj.getClass())
            return false;
        B other = (B) obj;
        if (num != other.num)
            return false;
        if (model == null) {
            if (other.model != null)
                return false;
        } else if (!model.equals(other.model))
            return false;
        return true;
    }

    

}

public class Demo {
        public static void main(String[] args) {
            B obj = new B();
            obj.num = 3;
            obj.model = "model3";

            B obj1 = new B();
            obj1.num = 3;
            obj1.model = "model3";

            System.out.println(obj); //B [num=3, model=model3]

            System.out.println(obj.equals(obj1)); //true
        }
}

```

# class upcasting from sub to super class
```
class A {
    public void showA(){
        System.out.println("show B");
    }

}
class B extends A{

    public void showB(){
        System.out.println("show B");
    }
}

public class Demo {
        public static void main(String[] args) {
            //upcasting from sub to super class
           A obj = (A) new B();
            // obj.showB(); Obj type class A cannot access type class B method
            
            //downcasting from super to sub
            B obj1 = (B) obj;
            obj1.showB(); //show B
        }
}
```

# not everything in Java is oop, e.g. primitive value
Collection only uses wrapper class;

int -> Integer
char -> Character
double -> Double

Autoboxing and autounboxing is to take wrapper out of wrapper class

```
public class Demo {
        public static void main(String[] args) {
           int num = 7;
           //boxing - take primitive value into a reference var
           Integer num1 = new Integer(num); //autoboxing
           Integer num2 = num;

           //unboxing - take obj to make it primitive
           int num3 = num2;

           //parseInt convert string to num
            String str = "12";
            int num4 = Integer.parseInt(str); //12

           System.out.println(num4); //7
        }
}
```

# abstract is a design template without details
Method name without implementation details

Abstract class can have abstract method or normal method
- if sub class extends abstract class, it must override the abstract methods

you cannot create object out of abstract class

# concrete class vs. abstract class
concrete class extends from abstract class

You can create object from concrete class, not abstract class

# inner class

```
class A{
    int age;

    public void showA(){
        System.out.println("Show A");
    }

    class B {
        public void showB(){
            System.out.println("show B");
        }
    }
}

public class Demo {
        public static void main(String[] args) {
          A obj = new A();
          obj.showA(); //Show A

          //Below means B is an inner class of A
          //We have to use instance of A to create an object from class b
          A.B obj1 = obj.new B();
          obj1.showB(); //Show B
        }
}

```

```
class A{
    int age;

    public void showA(){
        System.out.println("Show A");
    }

    //make class B static
    static class B {
        public void showB(){
            System.out.println("show B");
        }
    }
}

public class Demo {
        public static void main(String[] args) {
          A obj = new A();
          obj.showA(); //Show A

          //Below means B is an inner class of A
          //B is a static class, we can access the class from A directly
          A.B obj1 = new A.B();
          obj1.showB(); //Show B
        }
}
```

# anonymous inner class - without classname, create an object with new implementation
```
class A{
    int age;

    public void showA(){
        System.out.println("Show A");
    }
}

public class Demo {
        public static void main(String[] args) {
          A obj = new A(){
            //anonymous inner class
            public void showA(){
                System.out.println("Anonymous inner class");
            }
          };
          obj.showA(); //Anonymous inner class
        }
}
```

# use abstract class with anonymous inner class
```
//abstract class with abstract method
abstract class A{
     public abstract void show();
}

public class Demo {
        public static void main(String[] args) {
          A obj = new A(){
            //inner class to implement abstract details
            public void show(){
                System.out.println("Anonymous inner class");
            }
          };
          obj.show(); //Anonymous inner class
        }
}

```

# what is interface
interface is not a class, and it comes with public abstract key word

```
//interface is by default is public abstract
//you cannot instantiate from interface
//interface will tell u what methods to have, no implementation
interface A{
    int age = 34; //every var in interface is final and static, we have to declare it
    String name = "Tom"; 
    void show();
    void config();
}

//var in interface is final and static
//we say class B implements A's method body
class B implements A{
    public void show(){
        System.out.println("Show B");
    }
    public void config(){
        System.out.println("Config B");
    }
}

public class Demo {
        public static void main(String[] args) {
         A obj;
         obj = new B();
         obj.show();
         obj.config();
         //we cannot change the var in interface because it is final
        //  obj.age = 34;
         System.out.println(obj.age); //34
        }
}

```

# We can 1 class to implement multiple interface; we can only have 1 class extends 1 abstract class

we can also extends an interface to another interface

```

interface A{
    void show();
    void config();
}

interface C extends  A{
    void showC();
}
class B implements C{
    public void show(){
        System.out.println("Show B");
    }
    public void config(){
        System.out.println("Config B");
    }
    public void showC(){
        
    }
}

public class Demo {
        public static void main(String[] args) {
         A obj;
         obj = new B();
        }
}

```

```
class -> class -> extends
interface -> interface -> extends
class -> interface -> implement
```

# make your app loosely coupled, we can use interface type
```

//interface is more like a very very general type
//it is a type like ts to restrict
interface Computer{
    void code();
}

class Desktop implements Computer{
    public void code(){
        System.out.println("code in desktop");
    }
}

class Laptop implements Computer{
    public void code(){
        System.out.println("code in laptop");
    }
}

class Developer{
    public void devApp(Computer lap){
        lap.code();
    }
}

public class Demo {
        public static void main(String[] args) {
            //we use Computer interface type to contain both laptop and desktop
            Computer desk = new Desktop();
            Developer jerry = new Developer();
            jerry.devApp(desk);//code in desktop
        }
}

```

# enum is a class weird, but it have pre default objects
enum is named constants

```
enum Status{
    Running, Failed, Pending, Success
}
public class Demo {
        public static void main(String[] args) {
            System.out.println(Status.Running);
            System.out.println(Status.Running.ordinal()); //0
            System.out.println(Status.values()[0]);
        }
}
```

# enum is a class actually
java.lang.Enum
```

enum Status{
    Running, Failed, Pending, Success
}
public class Demo {
        public static void main(String[] args) {
            Status s = Status.Running;
            System.out.println(s.getClass().getSuperclass()); //class java.lang.Enum
        }
}

```

# we can use enum constructor to create objects
```

enum Laptop{
    Macbook(3000), Sony, Fuji(2000);
    private int price;

    private Laptop() {
        this.price = 500;
    }

    private Laptop(int price) {
        this.price = price;
    }

    public int getPrice() {
        return price;
    }

    public void setPrice(int price) {
        this.price = price;
    }

    
    
}
public class Demo {
        public static void main(String[] args) {
//             Macbook : 3000
// Sony : 500
// Fuji : 2000
            for(Laptop s: Laptop.values()){
                System.out.println(s + " : " + s.getPrice());
            }
        }
}
```

# annotation - metadata to supply extra info to runtime or compiler

below will tell me if we have overriden the method

```
@override
```

When start working on frameworks, we will use mostly many annotation

```
//this class is deprecated
@Deprecated

@SuppressWarning

and many more
```

# types of interface
1. normal - multi methods
2. functional (SAM - single abstract method) - only 1 method 
3. marker - interface with no methods e.g. save the object in your hard drive by serialize the object; when you load data from hard drive - it is deserialization. You can give permission for serialization in marker interface

# functional interface with annotation
We can use lamda expression only with functional interface

```
@FunctionalInterface //annotation only allows 1 method
interface A {
    void show();    
}

public class Demo {
        public static void main(String[] args) {
            A a = new A() {
                public void show(){
                    System.out.println("this is A");
                }
            };
            a.show();
        }
}

```

# Java only know it is very verbose, so create lamda (syntatically surgar reduced code)

Lamda expression only works with functional interface

```
@FunctionalInterface //annotation only allows 1 method
interface A {
    void show(int i);    
}

public class Demo {
        public static void main(String[] args) {
            //We do no need type for i below and no brackets also due to interface defining
            //lamda will do not create a new class file for it
            A a = i -> System.out.println("show " + i);
            a.show(3);
        }
}

```

# error
1. compile time error
2. runtime error
3. logical error

there are 2 types of statement
1. normal statement - int i = 8;
2. critical statement - maybe API call. 18/0 - arith erro - divide by zero. Execution stops if encounter error

```

public class Demo {
        public static void main(String[] args) {
            int i = 0;
            int j = 9;

            try {
                j = 8/i;
            } catch(Exception e){
                System.out.println("something went wrong");
            }

            System.out.println("bye" + j);
        }
}

```

# you can use multiple catch to catch different types of error, very good in large application
```

public class Demo {
        public static void main(String[] args) {
            int i = 1;
            int j = 9;
            String string = null;

            int[] nums = new int[5];

            try {
                j = 8/i;
                System.out.println(nums[1]);
                // System.out.println(nums[5]);
                System.out.println(string.length());
            } 
            //u can have multiple cath block to catch specific exceptions
            catch(ArithmeticException e){
                System.out.println("divide by zero error" + e);
            }
            catch (ArrayIndexOutOfBoundsException e){
                System.out.println("stay within yoiur limit");
            }
            //in the end we can capture everything, if we r not sure what is wrong
            //like default in switch
            catch (Exception e){
                System.out.println("something wrong" + e);
            }

            System.out.println("bye" + j);
        }
}

```

# Exception hierarchy
Object -> throwable ->
- error (thread death, IO error, virtual machine error, out of memory)
- exception 
1. Check exception - check at compile time, compulsory to catch it
runtime exception e.g. arithmetic, arrayindex, nullpointer, 
2. unchecked exception - did not check at compile time, mainly due to programming errors, often as result of logical errors, not compulsory to catch it

Source: https://www.udemy.com/course/spring-5-with-spring-boot-2/learn/lecture/35568674#overview

# throw vs. throws

 Throw error

 # what is throws??? duck the exception using throws keyword
 Duck the exception using throws - we avoid handling exceptions using throws key word
 so we only need to write try catch in the sub class, or individual super classes
```
class A {
    //throws here mean class A will not handle this error
    //it will be handled elsewhere
    public void show() throws ClassNotFoundException{
            Class.forName("Cal"); 
    }

}

public class Demo {
        public static void main(String[] args) {
            //we need to handle this checked exception
            try {
                A obj = new A();
                obj.show();
            } catch (ClassNotFoundException ex) {
                ex.printStackTrace();
            }
        }
}
```

# bufferReader takes in inputStreamReader
//this is old java way of doing this
```
public class Demo {
        public static void main(String[] args) throws IOException {
          System.out.println("Enter a number");

          InputStreamReader in = new InputStreamReader(System.in);
          BufferedReader br = new BufferedReader(in);

          int num = Integer.parseInt(br.readLine());
          System.out.println(num);
            br.close();
        }
}
```

# multi thread is usually handle by large framework itself e.g. java spring. usu we do not write it ourselves
```
//we have a scheduler in os to manage the execution of the thread

//once we extends thread, we make the obj an thread
class A extends Thread{
    public void run(){
        for(int i =0; i<6; i++){
            System.out.println("show A");
        }
    }
}

class B extends Thread{
    public void run(){
        for(int i =0; i<6; i++){
            System.out.println("show B");
        }
    }
}


public class Demo {
        public static void main(String[] args) {
          A a = new A();
          B b = new B();
          //start will trigger run methods in the class
          a.start();
          b.start();
        }
}

```

# thread priority and sleep
priority 5 is default, priorty 10 is the highest
using priority can only suggest to scheduler what to do first

sleep can ask scheduler to sleep one thread, move on to the next thread, but there is no way to guarantee it; we can only optimize

```

class A extends Thread{
    public void run(){
        for(int i =0; i<6; i++){
            System.out.println("show A");
            try {
                //ask scheduler to put thread to sleep
                Thread.sleep(10);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}

class B extends Thread{
    public void run(){
        for(int i =0; i<6; i++){
            System.out.println("show B");
            try {
                Thread.sleep(2);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}


public class Demo {
        public static void main(String[] args) {
          A a = new A();
          B b = new B();
          //start will trigger run methods in the class
        b.setPriority(Thread.MAX_PRIORITY);
          System.out.println(b.getPriority()); //5 is default, 10 is max
          a.start();
          try {
            Thread.sleep(10);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
          b.start();
        }
}

```

# runnable vs thread
Extends to thread, then cannot extend to other classs

we need another way to implement Runnable interface

```


class A implements  Runnable{
    public void run(){
        for(int i =0; i<6; i++){
            try {
                System.out.println("show A");
                Thread.sleep(10);
            } catch (InterruptedException ex) {
            }
        }
    }
}

class B implements  Runnable{
    public void run(){
        for(int i =0; i<6; i++){
            System.out.println("show B");
            try {
                System.out.println("show A");
                Thread.sleep(10);
            } catch (InterruptedException ex) {
            }
        }
    }
}


public class Demo {
        public static void main(String[] args) {
          Runnable a = new A();
          Runnable b = new B();

          Thread t1 = new Thread(a);
          Thread t2 = new Thread(b);
          t1.start();
          t2.start();
        }
}

```
# race condition
You can mutate variable value int i = 4;
String is immutable

If multiple thread trying to update int i; the state of i is unstable.

Thread safe - only 1 method to be executed 1 time

Thread is very unpredictable

source: https://github.com/navinreddy20/Javacode/blob/main/19.5%20Race%20Condition.java

to make sure 1 thread is only used after another, we can use `synchronized` key word on the method, so that this method can only be used by 1 single thread at a time

# thread state
Below are several states, some concepts to grasp; no need to remember now.
1) New state
2) runnable (after start())
3) running (run())
4) waiting (on hold)
5) Dead
source: https://github.com/navinreddy20/Javacode

# Collection API (concept), Collection (interface), Collections (class) - how to group the elements

we can use stack or queue, or different data structures.

# Collection is an interface, implemented by 
interable -> collection -> List/Queue/Set

Left side is interface and right is the class
For example, interface List extends Collection List; then class ArrayList extends List
1. List -> arrayList, Linkedlist
2. Queue -> Dequeue
3. Set -> Hashset, Linked Hashset

Map - what is it?? it does not extend to collection, but is part of the Collection API concept. -> HashMap class implements Map

The most mostly used is ArrayList and Hashset.

We can directly print a collection. not need to write toString ourselves.

Collection interface does not have get index method. List has get index method

```
import java.util.ArrayList;
import java.util.List;

public class Demo {
        public static void main(String[] args) {

            // Collection<Integer> nums = new ArrayList<Integer>();
            //Collection does not have get index method
            //Changed to List below
            List<Integer> nums = new ArrayList<Integer>();

            nums.add(3);
            nums.add(4);
            
            for (int n: nums){
                System.out.println(n);
            }

            System.out.println(nums.get(0)); //3
        }
}

```

# set supports only unique value
Set will not give values in sorted format, and there is no index value in set.

Collection of a unique values

TreeSet will give a sorted values
```
public class Demo {
        public static void main(String[] args) {

            //set only give unique values
            //does not give u sorted value
            Set<Integer> nums = new HashSet<Integer>();

            nums.add(3);
            nums.add(4);
            nums.add(3);
            
            for (int n: nums){
                System.out.println(n); //3 and 4
            }

        }
}

```

```
public class Demo {
        public static void main(String[] args) {

            //set only give unique values
            //does not give u sorted value
            Set<Integer> nums = new TreeSet<Integer>();

            nums.add(33);
            nums.add(43);
            nums.add(33);
            nums.add(11);
            
            for (int n: nums){
                System.out.println(n); //11.  33. 43
            }

        }
}
```

# interator is an interface before collection
```
public class Demo {
        public static void main(String[] args) {

            //set only give unique values
            //does not give u sorted value
            Collection<Integer> nums = new TreeSet<Integer>();
            nums.add(1);
            nums.add(2);
            Iterator<Integer> values =  nums.iterator();
            while(values.hasNext())
                System.out.println(values.next());
        }
}

```

# Map does not extend to Collection interface, but is part of Collection concept, Map is a key value pair collection

Keys cannot be repeated. Map is like a combination of set and list.

what is the difference between add and put?
- add - just add it
- put - try to add, if there is an existing one, just replace it

```

public class Demo {
        public static void main(String[] args) {

            Map<String, Integer> students = new HashMap<String, Integer>();
            students.put("John", 33);
            students.put("Tom", 43);
            System.out.println(students.keySet());

            for( String key: students.keySet()){
                System.out.println(key + ":" + students.get(key)); //Tom:43
                //John:33
            }
        }
}
```

# HashTable almost the same as HashMap.

HashTable is synchronized version - better to work with multiple threads.

# how to sort collection? using comparator vs comparable
- comparator is a function parameter for sort
- Comparable is an interface which has its own compareTo function

//sorting is easy by collections
Collections.sort(nums)

```
public class Demo {
        public static void main(String[] args) {

          List<Integer> nums = new ArrayList<>();
          nums.add(43);
          nums.add(31);
          nums.add(72);
          nums.add(29 );
          //quite easy to sort
          //you can access the funtion directly className.methodName, if it is a public static method
          Collections.sort(nums);
          System.out.println(nums);
        }
}

```

# customize sorting, we have to pass in a comparator
```
public class Demo {
        public static void main(String[] args) {

          List<Integer> nums = new ArrayList<>();
          nums.add(43);
          nums.add(31);
          nums.add(72);
          nums.add(29 );
            
          //Comparator is an interface and we are implementing anonymous class here
          Comparator<Integer> com =  new Comparator<Integer>() {
                public int compare(Integer i, Integer j){
                    if(i%10 > j%10)
                        return 1;
                    else 
                        return -1;
                }
            };
          Collections.sort(nums, com); 
          System.out.println(nums); //[31, 72, 43, 29]
        }
```

# Comparator for obj

```
class Student{
    int age;
    String name;

    public Student(int age, String name) {
        this.age = age;
        this.name = name;
    }

    @Override
    public String toString() {
        return "Student [age=" + age + ", name=" + name + "]";
    }
    
    
}

public class Demo {
        public static void main(String[] args) {

          List<Student> students = new ArrayList<>();
          students.add(new Student(31, "Tom"));
          students.add(new Student(11, "Tom"));
          students.add(new Student(22, "Tom"));
          students.add(new Student(23, "Tom"));
        
            
          Comparator<Student> com =  new Comparator<Student>() {
                public int compare(Student i, Student j){
                    if(i.age > j.age)
                        return 1;
                    else 
                        return -1;
                }
            };
          Collections.sort(students, com); 

        //Without toString override in Student class, we cannot see the underlying obj
          System.out.println(students); 
        }
}
```

# use class to implement Comparable interface
```
class Student implements Comparable<Student>{
    int age;
    String name;

    public Student(int age, String name) {
        this.age = age;
        this.name = name;
    }

    @Override
    public String toString() {
        return "Student [age=" + age + ", name=" + name + "]";
    }

    //automatically this will be used when use Collections.sort(student obj)
    @Override
    public int compareTo(Student that) {
        return this.age > that.age? -1: 1;
    }   
    
}

public class Demo {
        public static void main(String[] args) {

          List<Student> students = new ArrayList<>();
          students.add(new Student(31, "Tom"));
          students.add(new Student(11, "Tom"));
          students.add(new Student(22, "Tom"));
          students.add(new Student(23, "Tom"));
        
        //sort will auto use the CompareTo function above when sorting    
        Collections.sort(students); 

        //Without toString override in Student class, we cannot see the underlying obj
          System.out.println(students); 
        }
}
```


# forEach method in iterable class
```
public class Demo {
        public static void main(String[] args) {

        List<Integer> nums = Arrays.asList(3,5,6,7,7);
        //forEach is part of iterable
        nums.forEach(n-> System.out.println(n));
        }   
}

//alternative way of writing
public class Demo {
        public static void main(String[] args) {

        List<Integer> nums = Arrays.asList(3,5,6,7,7);
        // Consumer<Integer> con = new Consumer<Integer>() {
        //     public void accept(Integer n){
        //         System.out.println(n);
        //     }
        // };

        Consumer<Integer> con = n-> System.out.println(n);
        nums.forEach(con);
        }   
}
```

# stream API - easier to manipulate array, but it will not change the original array
- filter function
- map function
- sort
- reduce

Note: you cannot operate on the stream more than once.

```
public class Demo {
        public static void main(String[] args) {

        List<Integer> nums = Arrays.asList(3,4,5,6,7,7);
        // Stream<Integer> s1 = nums.stream();
        // Stream<Integer> s2 = s1.filter(n-> n%2 ==0);
        // Stream<Integer> s3 = s2.map(n-> n*2);
        // int result = s3.reduce(0, (c,e)-> c+e);
        
        int result = nums.stream()
            .filter(n->n%2==0)
            .map(n->n*2)
            .reduce(0, (c,e)-> c+e);

        System.out.println(result);//20
        //u cannot operate on stream more than once 
        // s1.forEach(n-> System.out.println(n));
        // s1.forEach(n-> System.out.println(n));
        // s2.forEach(n-> System.out.println(n)); //6
        }   
}
```

# what is predicate a functional interface for filter func - a way to determine true or false

```
//  Predicate<Integer> p = (Integer t) ->  t%2 ==0;
        Predicate<Integer> p = new Predicate<Integer>() {

            @Override
            public boolean test(Integer t) {
                return t%2 ==0;
            }
            
        };

        int result = nums.stream()
            .filter(p)
            .map(n->n*2)
            .reduce(0, (c,e)-> c+e);
```

# Map needs a function, which has a method Apply, Function is a functional interface
```
 Function<Integer, Integer> func = new Function<Integer,Integer>() {

            @Override
            public Integer apply(Integer t) {
                return t*2;
            }
            
        };

        int result = nums.stream()
            .filter(n->n%2==0)
            .map(func)
            .reduce(0, (c,e)-> c+e);

        System.out.println(result);//20
```

# you can use sorted for stream as well
```
        Stream<Integer> sortedValues = nums.stream()
            .filter(n->n%2==0)
            .sorted();

        sortedValues.forEach(n-> System.out.println(n));
```

# when you use filter to each element, you can use parallelStream for faster filtering

mapToInt
```
public class Demo {
        public static void main(String[] args) {
        int size = 10_000;
        List<Integer> nums = new ArrayList<>(size);
        Random ran = new Random();
        for (int i=0; i<=size; i++){
            nums.add(ran.nextInt(100));
        }
        // System.out.println(nums);
        int sum1 = nums.stream()
                        .map(i-> i*2)
                        .reduce(0, (c,e)-> c+e);

        int sum2 = nums.stream()
                        .map(i-> i*2)
                        //map to int convert all value to int
                        //intStream has a method called .sum()
                        .mapToInt(i->i)
                        .sum();

        System.out.println(sum1 + " " + sum2);

    }
}
```

```
        int sum1 = nums.stream()
                        .map(i-> i*2)
                        .reduce(0, (c,e)-> c+e);
        //parallel stream is faster on large n
        //however, for small n, parallel is slower because parallel has to create multiple streams
        int sum2 = nums.parallelStream()
                        .map(i-> i*2)
                        .mapToInt(i->i)
                        .sum();

```

source: https://www.udemy.com/course/spring-5-with-spring-boot-2/learn/lecture/41113474#overview

# Optional class in java, intro in java 1.8
Try to avoid null pointer exception
```
public class Demo {
        public static void main(String[] args) {
       List<String> names = Arrays.asList("Navin", "yz");

       Optional<String> name = names.stream()
                            .filter(str-> str.contains("x"))
                            .findFirst();

        //each element has a get() method
        //null pointer does not exist, we will get NoSuchElementException due to Optional
        // System.out.println(name.get());

        //if there is no such value, we can return "Not found"
        System.out.println(name.orElse("Not found"));
        
        //another way of doing, we can add orElse and remove Optional
        String name2 = names.stream()
                            .filter(str-> str.contains("x"))
                            .findFirst()
                            .orElse("not found");
    }
}
```

# method reference in function that takes a call back function. class::methodName
```
public class Demo {
        public static void main(String[] args) {
       List<String> names = Arrays.asList("Navin", "yz");
        
       List<String> unames = names.stream()
            //convert to a new stream
            .map(n-> n.toUpperCase())
            //convert back to List
            .toList();
        System.out.println(unames); //[NAVIN, YZ]
        
        List<String> unames2 = names.stream()
        //Class::methodName -> method reference
        .map(String:: toUpperCase)
        .toList();

        System.out.println(unames);
        //class.object::method
        unames2.forEach(System.out::println);

    }
}
```

# constructor reference - use constructor in another function
```
import java.util.Arrays;
import java.util.List;

class Student{
    String name;
    int age;
    public Student(String name) {
        this.name = name;
    }
    @Override
    public String toString() {
        return "Student [name=" + name + ", age=" + age + "]";
    }

    
}

public class Demo {
        public static void main(String[] args) {
       List<String> names = Arrays.asList("Navin", "yz");
        
       List<Student> students = names.stream()
                                .map(s-> new Student(s))
                                .toList();

                
        System.out.println(students);

        //use constructor reference
        List<Student> students2 = names.stream()
        .map(Student::new)
        .toList();

        System.out.println(students2);

    }
}
```



