# SOLID PRINCIPLES EXPLANATION FOR EVERYONE!!

## 1) What are the solid principles?

ok l khawa , simply hado li principl ndaro by nerds as result of years of developing , the first goal of hado les principl howa ano tkhdm software yji sahel from the side of maintenance and expansion , ida matb3tsh hado les principl mostly your program , yji kach wa7d ymodfiy 7aja brk l app ga3 tkoli ,

### S-O-L-I-D stands for :

- `S` - Single Responsibility Principle
- `O` - Open-Closed Principle
- `L` - Liskov Substitution Principle
- `I` - Interface Segregation Principle
- `D` - Dependency Inversion Principle

## S - Single Responsibility Principle (SRP)

"class wa7da , 3andha job wa7d " -ana-

3andna student class as folowing:

```js
class Student {
  constructor(name, studentId, grades) {
    this.name = name;
    this.studentId = studentId;
    this.grades = grades;
  }

  calculateAverage() {
    // Calculating grades  ok nice
    return this.grades.reduce((a, b) => a + b, 0) / this.grades.length;
  }

  saveToDatabase() {
    // Database operations  deja 3andna job l student m3a l grdes 3lah nzidlo DB ?
    console.log(`Saving ${this.name} to database...`);
    // Database logic here
  }

  sendEmailReport() {
    // student 3ando khdma m3a grades 3lah  nzid nmdlo mail? //
    console.log(`Sending report to ${this.name}@school.com...`);
  }

  generateReportCard() {
    //  STUDENT 3ANDO KHDMA M3A GRADES 3LAAAH NIZD NMDLO KHDMA REPEROTS !! //
    return `Report Card for ${this.name}\nAverage: ${this.calculateAverage()}`;
  }
}
```

#### Better Approach: Separate Each Responsibility into Its Own Class

1. student class

```js
class Student {
  constructor(name, studentId, grades) {
    this.name = name;
    this.studentId = studentId;
    this.grades = grades;
  }

  calculateAverage() {
    if (this.grades.length === 0) return 0;
    return this.grades.reduce((a, b) => a + b, 0) / this.grades.length;
  }
}
```

2. responsiblty ta3 DB in new class `StudentRepository`:

```js
class StudentRepository {
    save(student) {
        console.log(`Saving ${student.name} to database...`);
        // bla bla bla  db

    }
```

3. responsiblty ta3 EMAIL in new class `EmailService`:

```js
class EmailService {
  sendReportCard(student, reportCard) {
    console.log(`Sending email to ${student.name}@school.com`);
    console.log(reportCard);
  }
}
```

4. responsiblty ta3 REPORTS in new class `ReportCardGenerator`:

```js
// didcas
class ReportCardGenerator {
   console.log("card")
}

```

## O - Open-Closed Principle

"kmlt khdm class? function? feature? l79t l point win rak 7ab tzid kach 7aja? BALAK TBDL L CODE LI DERTO DEJA" -ana-

Well you see:
`Objects or entities should be open for extension but closed for modification.` Which means la class tkon extendable bla ma tbdlf code ta3ha for example:

```js
// example machi mli7 par example lokan na7b nzid shape lazem nbdl f code o nzid IF
class AreaCalculatorBad {
  calculateArea(shape) {
    if (shape.type === "circle") {
      return Math.PI * shape.radius ** 2;
    } else if (shape.type === "rectangle") {
      return shape.width * shape.height;
    } else if (shape.type === "triangle") {
      return (shape.base * shape.height) / 2;
    }
  }
}
```

### Better Approach

```java
// nkhdmo abstract class shape mnha ndiro l calc ta3 l area
// Create an abstract Shape class with area calculation
abstract class Shape {
    // Abstract method - must be implemented by subclasses
    public abstract double calculateArea();
}



class AreaCalculator {
    public double calculateArea(Shape shape) {
        return shape.calculateArea();
    }
}
// doka just akhdm class for each new shape o dir extend simply
// Now just create a class for each new shape and extend it

class Circle extends Shape {
    private double radius;

    public Circle(double radius) {
        this.radius = radius;
    }

    @Override
    public double calculateArea() {
        return Math.PI * radius * radius;
    }
}


class Rectangle extends Shape {
    private double width;
    private double height;

    public Rectangle(double width, double height) {
        this.width = width;
        this.height = height;
    }

    @Override
    public double calculateArea() {
        return width * height;
    }
}


class Triangle extends Shape {
    private double base;
    private double height;

    public Triangle(double base, double height) {
        this.base = base;
        this.height = height;
    }

    @Override
    public double calculateArea() {
        return (base * height) / 2;
    }
}

// o hna nzid for example hexagon cause why not :

class Hexagon extends Shape {
    private double width;
    private double height;

    public Hexagon(double width, double height) {
        this.width = width;
        this.height = height;
    }

    @Override
    public double calculateArea() {
     // idk tbh
    }
}

```

## L - Liskov Substitution Principle

'ida nta ta3rf tkhdm website , ur sons lazem ga3 ya3rfo ykhdmo les website' -kach bnadem-

Imagine you have class A, and you have class B extends A. In this principle ta9dr tst3ml B f ay makan knt tsta3ml A bla ma tbdl l code li derto 3la A

Imagine you have class bird, and class penguin extends bird. In this case penguin ma t9drch tsta3ml f makan li knt tsta3ml f bird 7it penguin ma y9drch y3ml fly (البطريق ما يطيرش), donc madernach

### Example:

```java

abstract class Bird {
    public abstract void fly();
}

class Eagle extends Bird {
    @Override
    public void fly() {
        System.out.println("Eagle is flying.");
    }
}
class Penguin extends Bird {
    @Override
    public void fly() {
      // may9drch ytir
      System.out.println("Penguin can't fly.");
    }
}



public class Main {
    public static void main(String[] args) {
        Bird eagle = new Eagle();
        Bird penguin = new Penguin();

        eagle.fly();
        penguin.fly(); // man9drch n7at penguin f planet eagle for example because may9drch ytir .
    }
}

```

### Solution

Basically, na7km l option ta3 fly li rahi f bird o n7atha f interface wa7dha

```java
abstract class Bird {
    protected String name;

    public Bird(String name) {
        this.name = name;
    }

    public void eat() {
        System.out.println(name + " is eating.");
    }

    public void makeSound() {
        System.out.println(name + " is making a sound.");
    }
}


interface Flyable {
    void fly();
}

class Sparrow extends Bird implements Flyable {
    public Sparrow() {
        super("Sparrow");
    }

    @Override
    public void fly() {
        System.out.println(name + " flies by flapping wings rapidly!");
    }
}

class Eagle extends Bird implements Flyable {
    public Eagle() {
        super("Eagle");
    }

    @Override
    public void fly() {
        System.out.println(name + " soars high in the sky!");
    }
}

class Penguin extends Bird  {
    public Penguin() {
        super("Penguin");
    }

    public void slide() {
        System.out.println(name + " slides on its belly!");
    }
}

```

(2000 years later like I didn't touch that for a month, and you would notice changes in the writing style because I changed a lot)

## Interface Segregation Principle (ISP)

"The reason you use arch linux probably" -me-

This principle is all about avoiding having big interfaces. The rule is clear: when we make an interface, it should be small and minimalist. The reason for this basically is to avoid forcing classes to use methods they don't need. In simple terms: avoid bloat.

example :

```ts
// You have this bloated (fat) interface
interface WarehouseWorker {
  pickItems(): void;
  packOrder(): void;
  generateInvoice(): void;
  scheduleTrucks(): void;
}

// If I wanted to use it on a class for example:

class Picker implements WarehouseWorker {
  pickItems() {}
  packOrder() {} // forced tkhdm biha
  generateInvoice() {} // same
  scheduleTrucks() {} // same
}
```

So try to keep everything small and optimized:

```ts
interface OrderPicker {
  pickItems(): void;
}

interface OrderPacker {
  packOrder(): void;
}

interface Accountant {
  generateInvoice(): void;
}

interface LogisticsManager {
  scheduleTrucks(): void;
}
```

And now you can use what you need exactly without any bloat:

```ts
// sa7a 3idek btw (nice right?)
class Picker implements OrderPicker {
  pickItems() {}
}
```
[Source lol](https://medium.com/@ramdhas/4-interface-segregation-principle-isp-solid-principle-39e477bae2e3)

## Dependency Inversion Principle (DIP)

"CEO never deals with machines in a factory, always he deals with that guy"

The concept basically is: imagine that you are a CEO of a very big company that has a factory. As a CEO of big company, it doesn't make sense to know every detail about every machine or procedure in a company and how it works. Simply he deals with intermediates, that will give him enough abstractions of the workflow so he can lead and make business decisions.

The same logic applies in code: you have high level components (the ones that deal with the business logic), and low level components (the ones that deal with the database queries and APIs).

The DIP says that you should have abstractions in between. Why so? Because imagine if your high level components make the queries directly. Imagine tomorrow you [Migrate](https://www.prisma.io/dataguide/types/relational/what-are-database-migrations) to another database. The entire system will crash on your face. 


  for example :

``` ts
// The low level component that deals with the database
class MongoDB {
  save(order: any) { }
}

// The high level component 
class OrderService {
  private database = new MongoDB();

  createOrder(order: any) {
    this.database.save(order);
  }
}
  
  ``` 

If you change from Mongo to another database, you have to manually change it.

**Better Approach:**

``` ts
interface Database {
  save(data: any): void;
}

// Low-level module (the detail)
class PostgresDB implements Database {
  save(data: any) {
    console.log("Saving to Postgres...");
  }
}

// High-level module (the logic)
class OrderService {
  private db: Database;

  // We "inject" the dependency via the constructor
  constructor(db: Database) {
    this.db = db;
  }

  createOrder(order: any) {
    this.db.save(order);
  }
}
```

---

**Hmmmm.... Inject? Dependency Injection? .... `To be continued`**
