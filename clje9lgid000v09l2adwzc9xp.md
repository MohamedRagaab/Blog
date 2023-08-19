---
title: "Object-Oriented Modeling (Module 2)"
datePublished: Tue Jun 27 2023 12:29:14 GMT+0000 (Coordinated Universal Time)
cuid: clje9lgid000v09l2adwzc9xp
slug: object-oriented-modeling-module-2
tags: object-oriented-modeling-module-2

---

This is the second module in the first course of the Software Design and Architecture Specialization and in this module, I'm going to cover the major design principles that are used in object-oriented modeling.

### Creating Models in Design

It is important when working on a software development project not to jump right into creating code to solve the problem. Instead, making the right product involves understanding the full requirements of your product and using good design.

One approach to help make the design process easier is the **object-oriented approach**. This allows for the description of concepts in the problem and solution spaces as objects—objects are a notion that can be understood by both **users** and **developers** because object-oriented thinking applies to many fields.

The goal during software design is to construct and refine “models” of all the objects of the software. Categories of objects involve:

• **entity objects**, where the initial focus during the design is placed in the problem space

• **control objects** that receive events and co-ordinate actions as the process moves to the solution space

• **boundary objects** that connect outside services to your system, as the process moves towards the solution space

Software models are often expressed in a visual notation, called Unified Modelling Language (**UML**).

### Four Design Principles

As described in module 1, object-oriented programming allows you to create models of how objects are represented in your system. However, to create an object-oriented program, you must examine the major design principles of such programs. Four of these major principles are **abstraction**, **encapsulation**, **decomposition**, and **generalization.**

**Abstraction**

Abstraction is one of the main ways that humans deal with complexity. It is the idea of simplifying a concept in the problem domain

The essential **characteristics** of abstraction can be understood in two ways: through basic **attributes** and basic **behaviors** or **responsibilities**

**Encapsulation**

This principle involves a concept that allows something to be **contained in a capsule**, some of which you can access from the **outside** and some of which you cannot.

There are **three ideas** behind **encapsulation**. These are:  
• The ability to “**bundle**” **attribute** values (or data) and **behaviors** (or functions) that manipulate those values, into a **self-contained object**.  
• The ability to “**expose**” certain **data** and **functions** of that object, which can be **accessed** from other **objects**, usually through an **interface**.  
• The ability to “**restrict**” access to certain **data** and **functions** to only within the object.

**Decomposition**

It consists of taking a **whole thing** and dividing it into **different parts**. The general rule for decomposition is to look at the different responsibilities of a whole and evaluate how the whole can be separated into parts that each have a specific responsibility.

**Generalization**

Generalization helps **reduce redundancy** when solving problems. It is a common principle used in many disciplines outside of software development

In object-oriented modeling, **generalization** is a main design principle, but beyond creating a **method** that can be applied to different data, object-oriented modeling achieves generalization by classes through **inheritance**. In generalization, we take repeated, common, or shared characteristics between two or more classes and factor them out into another class.

### Design Structure in Java and UML Class Diagrams

The design process, as explained in Module 1 of this course, consists of both the **conceptual design** and the **technical design**. Conceptual design, including prototyping and simulating higher-level designs, can be visualized through **CRC cards**. CRC cards make it easy to communicate with clients, and they allow you to create designs without the distraction of code. However, to guide technical design, a more sophisticated technique that can communicate your needs clearly to developers is needed. One technique used for technical design is that of a **UML class diagram**, also known as simply a class diagram. These class diagrams provide **more detail than CRC cards** and allow for easier conversion to classes for coding and implementation.

**Abstraction**

Let us look at how a CRC card might translate into a class diagram. Below is an **example of a CRC card**, as abstracted for a food item in the context of a grocery store.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1687855144239/3fbd5e86-563e-4961-84ef-d553175a4d41.png align="center")

Here is the same concept as a **class diagram**.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1687855155497/686bb536-5df0-4894-a98a-fc74397fc2bd.png align="center")

Every concept or class in a **class diagram** is represented with a **box**, as above. You will notice **three** sections in the box.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1687855244434/e6efa69b-2d1e-4893-be32-7443c2acd773.png align="center")

Drawing on our food example, we can see how easy it is to turn a class diagram into a class in **Java**

```java
public class Food {
 public String groceryID;
 public String name;
 public String manufacturer;
 public Date expiryDate;
 public double price;
 public boolean isOnSale( Date date ) { }
}
```

**Encapsulation**

UML class diagrams can express encapsulation. The class diagram itself already bundles data and functions in a self-contained object. However, access and restriction (two aspects of visibility) can be represented as well, through the use of symbols **–** and **+**. Below is an example of a UML class diagram for a student.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1687855583888/bfb46830-6824-4a56-a598-0a14ca58ca13.png align="center")

In this example, the attributes gpa and degreeProgram are hidden from **public** accessibility, as indicated by the **minus sign** (-). In other words, the minus sign indicates that a method or attribute is **private** and can only be accessed from within the class. On the other hand, the operations are public, as indicated by the **plus sign** (+). In other words, the plus sign indicates that a method can be accessed **publicly**.  
You may also notice the use of a **\# symbol**. This symbolizes that the Animal’s attributes are **protected**. Protected attributes in Java can only be accessed by:  
• the encapsulated class itself  
• all subclasses  
• all classes within the same package

**Decomposition**

The design principle of **decomposition** takes a whole thing and **divides** it into different parts. It also does the reverse and takes separate parts with different functionalities, and combines them to form a whole. There are three types of relationships in decomposition, which define the interaction between the whole and the parts:  
**• Association  
• aggregation  
• composition**

**1- Association**

indicates a **loose** relationship between two objects, which may interact with each other for some time. They are **not dependent** on each other—if one object is **destroyed**, the other can continue to **exist**, and there can be any number of each item in the relationship.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1687856551493/1b66be03-f12c-4b1e-b9a1-9425bea5d5f8.png align="center")

```java
public class Student {
 public void play( Sport sport ){
    execute.play( sport );
 }
}
```

In this code excerpt, a student is passed a sport object to play, but the student does not possess the sport. It only interacts with it to play. The two objects are completely separate but have a **loose relationship**. Any number of sports can be played by a student and any number of students can play a sport.

**2- Aggregation**

**Aggregation** is a “**has-a**” relationship where a whole has parts that **belong to** it. Parts may be shared among wholes in this relationship. Aggregation relationships are typically weak, however. This means that although parts can belong to wholes, they can also exist independently. An example of an aggregate relationship is that of an airliner and its crew. The airliner would not be able to offer services without the crew. However, the airliner does not cease to exist if the crew leaves. The crew also does not cease to exist if they are not in the airliner.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1687864161364/587bd867-2518-477f-b872-d7d7b7ffca1b.png align="center")

The empty **diamond** indicates which object is considered **the whole and** not **the part** in the relationship.

```java
public class Airliner {
 private ArrayList<CrewMember> crew;
 public Airliner() {
 crew = new ArrayList<CrewMember>();
 }
 public void add( CrewMember crewMember ) {
  …
 }
}
```

In the **Airliner class**, there is a **list of crew** members. The list of crew members is initialized to be empty and a public method allows new crew members to be added. An airliner has a crew. This means that an airliner can have zero or more crew members.

**3- Composition**

**Composition** is one of the most **dependent** of the **decomposition relationships**. This relationship is an exclusive containment of parts, otherwise known as a **strong “has-a” relationship**. In other words, a whole cannot exist without its parts, and if the whole is **destroyed**, then the parts are **destroyed** too. In this relationship, you can typically only access the parts through its whole. Contained parts are exclusive to the whole. An example of a composition relationship is between a house and a room. A house is made up of multiple rooms, but if you remove the house, the room no longer exists.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1687864485754/f4336df5-da8c-42f8-8895-3c5347cadc3f.png align="center")

If the diamond is **filled in**, it symbolizes that the “**has-a**” relationship is **strong**. The two objects would cease to exist without each other. The one “**dot dot star**” indicates that there must be **one** or **more Room** objects for the **House** object.

```java
public class House {
 private Room room;
 public House(){
 room = new Room();
 }
}
```

**4- Generalization**

The design principle of **generalization** takes repeated, **common**, or **shared** characteristics between **two or more classes** and factors them **out** into another class, so that code can be **reused**, and the characteristics can be **inherited** by **subclasses**.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1687865013298/f32afb3b-6cf0-4365-8d94-dc4148e269b1.png align="center")

The solid-lined arrow indicates that two classes are connected by **inheritance**. The **superclass** is at the **head** of the arrow, while the **subclass** is at the **tail**.

```java
public abstract class Animal {
    protected int numberOfLegs;
    protected int numberOfTails;
    protected String name;
    public Animal( String petName, int legs, int tails ) {
      this.name = petName;
      this.numberOfLegs = legs;
      this.numberOfTails = tails;
    }
    public void walk() { … }
    public void run() { … }
    public void eat() { … }
}
```

Since the **Animal** class is a **generalization**, it should not be created as an object on its own. The keyword **abstract** indicates that the class **cannot be** **instantiated**. In other words, an Animal object cannot be created.

The Animal class is a **superclass**, and any class that **inherits** from the Animal class will have **its attributes** and **behaviors**.

```java
public class Lion extends Animal {
    public Lion( String name, int legs, int tails) {
     super( name, legs, tails );
    }
    public void roar() { … }
}
```

Classes can have **implicit constructors** or **explicit constructors**. Below is an example of an **implicit constructor**:

```java
public abstract class Animal {
    protected int numberOfLegs;
    public void walk() { … }
}
```

Below is an example of an **explicit constructor**:

```java
public abstract class Animal {
    protected int numberOfLegs;
    public Animal( int legs ) {
        this.numberOfLegs = legs;
    }
}
```

```java
public class Lion extends Animal {
    public Lion( int legs ) {
        super( legs );
    }
}
```

**Interface Inheritance**

Other languages like C++ support **multiple inheritance**. This is when a **subclass** can have **two or more superclasses**. Java addresses the **restriction** of single implementation inheritance by **offering interface inheritance**, another form of generalization. To understand that, first, some programming language and design notions need to be explained.

In Java, the keyword **interface** is used to indicate that one is being defined. The letter **“l”** is sometimes placed before an actual name to indicate an interface.

```java
public interface IAnimal {
    public void move();
    public void speak();
    public void eat();
}

public class Lion implements IAnimal {
/* Attributes of a lion can go here */
    public void move() { … }
    public void speak() { … }
    public void eat() { … }
}
```

Another advantage of interfaces relates back to multiple implementation inheritance. This is because inheriting from two or more superclasses can cause **data ambiguity** if a subclass inherits from two or more superclasses that have attributes with the same name or behaviors with the same method signature, then it is not possible to distinguish between them. As Java cannot tell which one is referenced, it does not allow for multiple inheritance to prevent data ambiguity.

Now that you have an introduction to **design principles** and their manifestation in technical diagrams, this course will now turn to examine more nuanced understandings of those same design principles.