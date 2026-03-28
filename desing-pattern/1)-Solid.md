# SOLID PRINCIPLS EXPLANATION FOR EVERYONE!!

## 1) What are the solid principls?

ok l khawa , simply hado li principl ndaro by nerds as result of years of devoloping , the first goal of hado les principl howa ano tkhdm software yji sahel from the side of maintaininace and expansion , ida matb3tsh hado les principl mostly your program , yji kach wa7d ymodfiy 7aja brk l app ga3 tkoli ,

### S-O-L-I-D stands for :

`S-Single-responsisblty Principle`
`O-Open-Closed Principle`
`L-Liskov Substitution Principle`
`I-Interface Segregation Principle`
`D-Dependency Inversion Principle`

## S-Single-responsisblty Principle (SPR)

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

#### better approach: na7km each job o na7to f class

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

## O-Open-Closed Principle:

"kmlt khdm class? function ? featuer ? l79t l point win rak 7ab tzid kach 7aja ? BALAK TBDL L CODE LI DERTO DEJA " -ana-

well u see :
`Objects or entities should be open for extension but closed for modification.` which means la class tkon extendable bla ma tbdlf code ta3ha par example :

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

### better approach :

```java
// nkhdmo abstract class shape mnha ndiro l calc ta3 l area
abstract class Shape {
    // Abstract method - must be implemented by subclasses
    public abstract double calculateArea();
}



class AreaCalculator {
    public double calculateArea(Shape shape) {
        return shape.calculateArea();
    }
}
// doka just akhdm class for each new  shape o dir extend simply

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

// o hna nzid par example hexagon cuase why not :

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

## Liskov Substitution Principle

'ida nta ta3rf tkhdm website , ur sons lazem ga3 ya3rfo ykhdmo les website' -kach bnadem-

doka imagin 3and class A , o 3andk class B extends A , soo 7na f hada l principle ta9dr tst3ml B f ay makan knt tsta3ml A bla ma tbdl l code li derto 3la A

imagin 3ndk class bird , o class penguin extends bird , f had l case penguin ma t9drch tsta3ml f makan li knt tsta3ml f bird 7it penguin ma y9drch y3ml fly (البطريق ما يطيرش) , donc madernach

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
class penguin extends Bird {
    @Override
    public void fly() {
      // may9drch ytir
         System.out.println("Penguin can't fly.");
    }
}



public class Main {
    public static void main(String[] args) {
        Bird eagle = new Eagle();
        Bird penguin = new penguin();

        eagle.fly();
        penguin.fly(); // man9drch n7at penguin f plaset eagle par examplw becuase may9drch ytir .
    }
}

```

### solution :

bassically , na7km l option ta3 fly li rahi f bird o n7atha f interface wa7dha

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

(2000 years later like i didnt touch that for a month , and u would notice changes in the writing style becise i changed a lot)

## Interface Segregation Principle (ISP)

"the resoan u use arch linux probbaly"-me-

this pricipl is all about avoiding having big interfaces , the rule is clear , when we a make an interface , it should be small and mininmalistc , and the resaon for this bassically to avoid forcing classes to use methhods they dont need in simple terms avoid bloat

example :

```ts
// u have this bloated (fat) interface
interface WarehouseWorker {
  pickItems(): void;
  packOrder(): void;
  generateInvoice(): void;
  scheduleTrucks(): void;
}

// if i wanted to use it on a class for example :

class Picker implements WarehouseWorker {
  pickItems() {}
  packOrder() {} // forced tkhdm biha
  generateInvoice() {} // same
  scheduleTrucks() {} // same
}
```

soo try to keep everthing small and optimaised :

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

and now u can u use what u need exactly with out any bloat :

```ts
// sa7a 3idek btw
class Picker implements OrderPicker {
  pickItems() {}
}
```
[Source lol](https://medium.com/@ramdhas/4-interface-segregation-principle-isp-solid-principle-39e477bae2e3)

## Dependency Inversion Principle (DIP)

"CEO never deals  with  machines in a factory , allwayes he deals with that guy"

the  say bassicaly , imagin that u r a ceo  of very big comapny that have a facotry ,  as a ceo it of big comapny it dosent make sense to know every deatils about every machin or procuder in a comapny and how it works , simpily he deals with intemediats , that will give him the enough abstaractions of the work flow soo he can lead  and make busssnes  descsions 

 the same logic apply in code , u have high lvl componenets (the ones that delas with the bussnies logic ) , and a low lvl componenets (the ones that delas with the db querys and API`s)  
 
  the DIP says that u should have  abstractions in between , why soo? becuse imagain if ur high lvl componenets make the querrys directly , imagin tomowrow u [Migrate](https://www.prisma.io/dataguide/types/relational/what-are-database-migrations)  to another db ? all the systeme will crash on u face 


  for example :

  ``` ts
   // the low lvl compnenet that delas wiht the  data base
  class MongoDB {
  save(order: any) {  }
}
   
   // the high lvl component 
class OrderService {
  private database = new MongoDB();

  createOrder(order: any) {
    this.database.save(order);
  }
}
  
  ``` 

if u change from mongo to another db  u have to manully change it 

now better approach :

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



hmmmmmmm.... inject ?  dependcy injection ? ..... ```to be contunied ```
