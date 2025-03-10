# How do you apply inheritance and polymorphism in your portfolio projects?
Powered by AI and the LinkedIn community
1
Benefits of inheritance and polymorphism
2
How to use inheritance in your projects
3
How to use polymorphism in your projects
4
Examples of inheritance and polymorphism in projects
5
Tips for applying inheritance and polymorphism
Be the first to add your personal experience

6
Here’s what else to consider
Top experts in this article
Selected by the community from 15 contributions. Learn more
Member profile image
Rasul H.
Senior Backend Developer | Consultant | 5x Microsoft Certified & MVP | Tech Author
View contribution

4
Member profile image
Scott Bouma
Curiosity. Creativity. Kindness.
View contribution

3
Member profile image
Priya Pandey
Front Office | Equities Trading | Founder of SPRN ROY Foundation
View contribution

3
1
Benefits of inheritance and polymorphism
Inheritance and polymorphism are useful for creating modular and scalable code that can handle complex problems. Inheritance allows you to define a parent class that can pass down its attributes and methods to child classes, which can inherit, override, or extend them. This way, you can avoid repeating code and create a clear structure of related classes. Polymorphism allows you to use the same method name for different classes, but implement different behaviors depending on the class or object. This way, you can write flexible and generic code that can work with different types of objects.

Seyed Alireza Hashemi

Add your perspective

(edited)

Inheritance is a way of static extensibility, this happens at the class level and thus provides a strong object family. Also, it allows us to create a hierarchy of classes.
Polymorphism is a way of dynamic extensibility, this happens at the object level and thus provides runtime interchangeability (dynamic binding). From an abstract point of view, inheritance is a step that allows the use of polymorphism. Because of class hierarchy, we can use Polymorphism, without Inheritance Polymorphism is meaningless. The benefits of both are OO structure with sustainable, reliable, and maintainable code. Additionally, another benefit of inheritance and polymorphism is the creation of different forms of abstractions.

…see more

4

Reply


Inheritance lets new classes reuse existing code, reducing repetition and improving code organization. Polymorphism allows different class objects to be treated as one, simplifying code and enhancing flexibility. 

For example, consider a "Vehicle" class for cars and bikes. Inheritance helps create "Car" and "Bike" classes that inherit shared features, saving time and ensuring consistency. 

With polymorphism, a single "Drive" function can work for both "Car" and "Bike" objects, even though they function differently. This makes code adaptable and facilitates adding new vehicle types. 

In summary, inheritance, and polymorphism enhance code efficiency and maintainability in programming.

…see more

2

Reply


What I find most useful is implementing a generic algorithm on the base class and stubbing out methods that vary based on instance type and implementing them on child classes. If you find yourself calling superclass methods on the subclass you are likely not designing well. All child classes should be overriding "thin" methods that return instance specific data to the calling superclass method

…see more

2

Reply


While polymorphism is quite useful in both static and dynamic forms, inheritance may be extremely harmful:
- provokes deep class hierarchies
- replaces flexible "has properties" requirement with rigid "inherits from"
Better use implementation of interfaces/traits and little to no inheritance.

…see more

1

Reply


Also it helps us to dynamically inject objects, for enabling testing the code, without even deploying the artifacts in the target environment.


Reply
2
How to use inheritance in your projects
To use inheritance in your projects, you need to identify the common attributes and methods that can be shared by different classes, and create a parent class that contains them. Then, you can create child classes that inherit from the parent class, and add or modify the attributes and methods as needed. For example, if you are building a project that involves different types of animals, you can create an Animal class that has attributes like name, age, and color, and methods like eat, sleep, and make_sound. Then, you can create subclasses like Dog, Cat, and Bird that inherit from Animal, and override or extend the make_sound method to produce different sounds.

Seyed Alireza Hashemi

Add your perspective


If you love to jump in and start writing code, lean into that - just start coding!  One key, though: write well-encapsulated functions with clear inputs and outputs.  Note: so far, no inheritance planning needed!

Here's the magic: as your code expands, you'll soon need to do a similar job as you've already done.  Bingo!  Create a base class with the already-encapsulated functionality, and inherit as needed.  

Yes, you'll have some rework overhead, but it will be no more than what you would have put into pre-planning architecture. But more importantly: you'll do inheritance in support of a real problem; rather than a theoretical architecture that solves problems which may never arise or which you don't yet understand enough to solve well.

…see more

3

Reply


Inheritance: More like genetic inheritance where as child inherits behaviour/traits of her parents. It is based on DRY principle.

Polymorphism: The literal meaning is “many forms.” That’s because we build methods with the same name but different functionality.

Inheritance or Polymorphism is a result of implementation of objects relationship. First analye what behaviour you need and their relationship. 

…see more

3

Reply


To use inheritance in projects, first, identify common attributes and behaviors among classes. Create a base class with these shared elements. Then, make specialized subclasses that inherit from the base class. This way, you can reuse code, reduce redundancy, and ensure consistency while building on existing functionality to save time and maintain clean, organized code.

…see more

2

Reply

(edited)

Better don't use it at all. Identify functionality needed by specific piece of logic and encode it as interface or trait, then make your types implement it. Proposed example of Animal, Cat, Dog hierarchy is much clearer if Animal is an interface. Or, better, decide what you want from animal and encode these requirements.

…see more

Reply
3
How to use polymorphism in your projects
To use polymorphism in your projects, you need to define a method name that can be used by different classes or objects, but implement different behaviors depending on the type or state of the object. You can use polymorphism in two ways: by overriding a method in a subclass, or by using a special parameter called self in a method. For example, if you are building a project that involves different types of shapes, you can create a Shape class that has a method called area that takes self as a parameter. Then, you can create subclasses like Circle, Square, and Triangle that inherit from Shape, and override the area method to calculate the area based on the shape's attributes. Alternatively, you can use the self parameter in the area method to check the type of the object and return the appropriate calculation.

Seyed Alireza Hashemi

Add your perspective


First of all this is more towards python using the "self" as the first parameter in methods of a class/object. The alternative option goes against the topic itself. It should not have even been suggested.


3

Reply


To use polymorphism in your projects, define a common interface or base class that multiple related classes can implement or inherit from. This allows different objects to be treated as instances of the common base class or interface, making your code more flexible and adaptable. You can then write functions or methods that accept objects of the base class/interface type, allowing you to work with different objects in a unified way. This simplifies your code and makes it easier to extend and maintain, as you can add new classes that adhere to the common interface without changing existing code. Polymorphism promotes code reusability and enhances the overall flexibility of your project.

…see more

3

Reply


There are two ways to do this: up front and emergent design. Up front design is when you draw out your class diagrams first and then move on to implementation. Emergent design is when you start with what you want to accomplish and start with Test Driven Development allowing your tests to define the class structure as you write readable code. This also allows for more refactoring and loose coupling. While starting with a well thought out class structure works in the classroom, in the business world it rarely is that cut and dry.

…see more

2

Reply


First of all, identify where you really need polymorphic behavior. Next, describe it in terms of type's properties you need. Lastly, encode these properties as interface or trait. If you have some large chunk of logic which needs to be shared between interface implementors, it's a clear sign that piece of logic may not belong there.
Alternatively, if your language supports tagged unions, prefer them when number of polymorphic behaviors is fixed and well-known.
The example in the section on the left is incorrect, as it ultimately bans any shapes which don't have area from joining group of Shape types

…see more

Reply
4
Examples of inheritance and polymorphism in projects
To give you some inspiration, here are some examples of how you can use inheritance and polymorphism in your portfolio projects. For a game project, you can create a Character class with attributes like name, health, and inventory, and methods like move, attack, and use_item. Then create subclasses like Player, Enemy, and NPC that inherit from Character and add or modify the attributes and methods as needed. You can also use polymorphism to create a Weapon class with a method called damage that takes self as a parameter and returns the damage value based on the weapon's type and attributes. For a web app project, you can create a User class with attributes like username, email, and password, and methods like login, logout, and edit_profile. Then create subclasses like Admin, Moderator, and Member that inherit from User and add or modify the attributes and methods as needed. You can also use polymorphism to create a Post class with a method called display that takes self as a parameter and returns the HTML code based on the post's type and content. Lastly, for a data analysis project you can create a DataSource class with attributes like name, format, and location, and methods like load, clean, and transform. Then create subclasses like CSV, JSON, and SQL that inherit from DataSource and override or extend the methods as needed. You can also use polymorphism to create a Model class with a method called fit that takes self as a parameter and returns the model object based on the data source and the algorithm.

We are evolving the way we reward top contributors. Learn more
Seyed Alireza Hashemi
Some ways to get started:
  - One time at work…
  - In my experience…
  - One thing I’ve found helpful…
Cancel
Add
Contributor profile photo
Rudy Sandoval
Follow
Principal Member of Technical Staff, Computer Science at Sandia National Laboratories


The principal benefit of polymorphism involves patterns where underlying behavior should change at runtime. Consider where this applies to your business logic.

Polymorphism can be leveraged via class inheritance or interface implementation. However, the power and flexibility of modern interfaces does seem to succeed most use cases for class hierarchies. Specifically in Java, the lack of multiple inheritance and availability of default methods really favors interfaces.

Good inheritance and polymorphic design should keep composition and the SOLID Principles in mind, especially if working with a Dependency Injection framework. Not every structural problem is solved with a class hierarchy.

…see more

1

Reply
5
Tips for applying inheritance and polymorphism
When applying inheritance and polymorphism to your portfolio projects, it's important to use inheritance when there is a clear hierarchy of classes that share common attributes and methods. Similarly, when you want to write generic code that can work with different types or states of objects, you should use polymorphism. Additionally, you should ensure that your classes, attributes, and methods have descriptive and consistent names. Furthermore, it's important to document your code with comments or docstrings and test it with different inputs and outputs. This will help you debug any errors or exceptions.

We are evolving the way we reward top contributors. Learn more
Seyed Alireza Hashemi
Some ways to get started:
  - One time at work…
  - In my experience…
  - One thing I’ve found helpful…
Cancel
Add
6
Here’s what else to consider
This is a space to share examples, stories, or insights that don’t fit into any of the previous sections. What else would you like to add?

We are evolving the way we reward top contributors. Learn more
Seyed Alireza Hashemi
Some ways to get started:
  - One time at work…
  - In my experience…
  - One thing I’ve found helpful…
Cancel
Add
Contributor profile photo
Paul Slusarz
Follow
Software Developer at CARFAX


The original Design Patterns book by the "Gang of Four" is critical of inheritance and polymorphism. In their opinion, these techniques introduce coupling to classes that makes it difficult to reason and easy to introduce bugs. The authors suggest using object composition instead.

One interesting mental puzzle (not from the book)  illustrating how inheritance can be counterintuitive is the question "is square a subclass of a rectangle." The intuitive answer from math is "yes," but following that route leads to unintended side effects of calling setWidth or setHeight methods on the square class. Changing one ends up affecting the other value, which is not what users of rectangle class expect, violating the Liskov Substitution Principle.

…see more


source:
https://www.linkedin.com/advice/0/how-do-you-apply-inheritance-polymorphism
