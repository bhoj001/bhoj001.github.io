---
layout: post
title: "Design pattern in python- part1 Introduction"
description: "Design pattern"
# tags: [sample post, link post]
# link: http://mademistakes.com  
published: true
authors: 'Bhoj Bahadur Karki'
name: 'Bhoj Bahadur Karki'
share: true
---
## We have used:
- Python 3.6+

## Assumption of audience
It is assumed that you(the audience) have a moderate understanding of programming language. Experience in python will be added advantage. 
## Design Patterns

Design patterns are everywhere. When we talk about design pattern the history starts with 4 people, name as gang of four, starts to write software development concepts that are constantly reused in software development. Design pattern are solution to general software engineering problem. They help to solve problem and make product better. They are not finished products in themself but rather a concepts to solve certain software development problems. This artilce present the design pattern in series of parts and tries to implement them using python programming language. The design pattern can be divided into 3 category and further sub category as shown below:

1. __Creational design pattern__ 
2. __Structural design pattern__
3. __Behaviour design pattern__

__Creational design pattern:__ It is the design pattern that deals with the creation of an object. This category has following patterns:

| Design pattern name | Description                                                                 |
|:--------------------|:---------------------------------------------------------------------------:|
| Factory method      | It creates and object without specifying exact class to create              |
| Abstract Factory    | It groups object factory that have common theme                             |
| BUilder             | It constructs complex objects by separating construction and representation |
| Singleton           | It lets only one instance to be created from a class                        |
| Prototype           | It creates an object by cloning an existing object                          |

<br>
__Structural design pattern:__ It is the design pattern that deals with creation of class structure such as inheritance and composition. It concerns class and object composition.This category has following patterns:

| Design pattern name | Description                                                                                 |
|:--------------------|:-------------------------------------------------------------------------------------------:|
| Adaptor             | It helps to interface between two incompatible classes with an adaptor class                |
| Bridge              | It decouples an abstraction from its implementation so that the two can vary independently. |
| Composite           | It zero or more similar objects so that they can be manipulated as single object.           |
| Decorator           | It adds feature to already existing method without changing it's calling structure          |
| Facade              | It provides simplified interface for larger body of code.                                   |
| Flyweight           | It reduces the cost of creating and manipulating a large number of similar objects.         |
| proxy               | It provides a placeholder for another object to control access and reduce complexity        |

<br>
__Behaviour design pattern:__ It is the design pattern that deals with suitable interaction between objects, how to privide lose coupling and how to provide flexibility to extend them in future.This category has following patterns:

| Design pattern name | Description |
|:-|:-:|
| Chain of responsibility | It assigns command to chain of processing objects |
| Command | It creates objects which encapsulates actions and parameter |
| Intrepreter | It implements a specialized language |
| Iterator | It accesses the elements of an object sequentially without exposing its underlying representation. |
| Mediator | It allows loose copuling between classes by being mediator who know about detail of both classes |
| Memento | It provides the ability to restore an object to its previous state (undo). |
| Observer | It implements publisher/subscriber pattern to allow the observer object to see change in events |
| State | It allows an object to change it's behavior when internal state changes |
| Strategy | It allows one of a family of algorithms to be selected on-the-fly at runtime. |
| Template method | implements an algorithm as an abstract class, allowing its subclasses to provide concrete behavior. |
| Visitor | It separates an algorithm from an object structure by moving the hierarchy of methods into one object. |


