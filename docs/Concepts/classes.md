---
id: classes
title: Classes
---


## Overview

The 4D language supports the concept of **classes**. In a programming language, using a class allows you to define an object behaviour with associated properties and functions. 

Once a class is defined, you can **instantiate** objects of this class anywhere in your code. Each object is an instance of its class. A class can `extend` another class, and then inherits from its properties and functions.

The class model in 4D is similar to classes in JavaScript, and based on a chain of prototypes.

### Class object

A class is an object itself, of "Class" class. A class object has the following properties and methods:

- `name` which must be ECMAScript compliant
- `superclass` object (optional, null if none)
- `new()` method, allowing to instantiate class objects.

In addtion, a class object can reference: 
- a `constructor` object (optional)
- a `prototype` object, containing named function objects (optional).

A class object is a shared object and can therefore be accessed from different 4D processes simultaneously.


### Property lookup and prototype

All objects in 4D are internally linked to a class object. When 4D does not find a property in an object, it searches in the prototype object of its class; if not found, 4D continue searching in the prototype object of its superclass, and so on until there is no more superclass.

All objects inherit from the class "Object" as their inheritance tree top class.

```4d
  //Class: Polygon
Class constructor
C_LONGINT($1;$2)
This.area:=$1*$2
 
 //C_OBJECT($poly)
C_BOOLEAN($instance)
$poly:=cs.Polygon.new(4;3)

$instance:=OB Instance of($poly;cs.Polygon)  
 // true
$instance:=OB Instance of($poly;4D.Object)
 // true 
```

When enumerating properties of an object, its class prototype is not enumerated. As a consequence, `For each` statement and `JSON Stringify` command do not return properties of the class prototype object. The prototype object property of a class is an internal hidden property.


## Class stores

Available classes are accessible from their class stores. The following class stores are available:

- a class store for built-in 4D classes. It is returned by the `4d` command.
- a class store for each opened database or component. It is returned by the `cs` command. These are "user classes".

For example, you create a new instance of an object of myClass using the `cs.myClass.new()` statement (`cs` means *classtore*).


## Creating a user class

### Class file

A user class in 4D is defined by a specific method file (.4dm), stored in the `/Project/Sources/Classes/` folder. The name of the file is the class name.

> The class file name must be ECMAScript compliant. **Class names are case sensitive**.

For example, if you want to define a class named "Polygon", you need to create the following file:

- Database folder
	+ Project
		* Sources
			- Classes
				+ Polygon.4dm

> Class files are automatically created by 4D at the appropriate location when you create a class from the 4D Developer interface (**New > Class...** from the **File** menu, **New Class...** from the Explorer contextual menu -- see XXXX).


### Class contents

A user class file defines a model of object that can be instantiated in the database code by calling the `new()` class member method.
You will usually use specific [class keywords](#class-keywords), [class commands](#class-commands), as well as the `This` keyword in the class file.

For example:

```4d  
//Class: Person.4dm
Class constructor
  C_TEXT($1) // FirstName
  C_TEXT($2) // LastName
  This.firstName:=$1
  This.lastName:=$2
```

In a method, creating a "Person":

```
C_OBJECT($o)
$o:=cs.Person.new("John";"Doe")  
// $o: {firstName: "John";lastName: "Doe" }
```

Note that you could create an empty class file, and instantiate empty objects. For example, if you create the following `Empty.4dm` class file: 

```4d  
//Empty.4dm class file
//Nothing
```

You could write in a method:

```4d
$o:=cs.Empty.new()  
//$o : {}
$cName:=OB Class($o).name //"Empty"
```


## Class keywords

Specific 4D keywords can be used in class definitions:

- `Function <ClassName>` to define member methods of the objects. 
- `Class constructor` to define the properties of the objects (i.e. the prototype).
- `Class extends <ClassName>` to define inheritance.


### Class Function

#### Syntax

```
Function \<name>
// code
```

Class functions are properties of the prototype object of the owner class. They are objects of the "Function" class. 

In the class definition file, function declarations use the `Function` keyword, and the name of the function. The function name must be ECMAScript compliant.

Within a class function, the `This` is used as the object instance. For example:

```4d  
Function getFullName
  C_TEXT($0)
  $0:=This.firstName+" "+Uppercase(This.lastName)
 
Function getAge
  C_LONGINT($0)
  $0:=(Current date-This.birthdate)/365.25
```
  
For a class function, the `Current method name` command returns: "<ClassName>.<FunctionName>", for example "MyClass.myMethod".

In the database code, class functions are called as member methods of the object instance and can receive parameters if any. The following syntaxes are supported:

- use of the `()` operator. For example `myObject.methodName("hello")`.
- use of a "Function" class member methods
	- `apply()`
	- `call()`


> **Thread-safety warning:** If a class function is not thread-safe and called by a method with the "Can be run in preemptive process" attribute:  
> - the compiler does not generate any error (which is different compared to regular methods),
> - an error is thrown by 4D only at runtime.


#### Example

```
// Class: Rectangle
Class Constructor
	C_LONGINT($1;$2)
	This.name:="Rectangle"
	This.height:=$1
	This.width:=$2
  
// Function definition
Function getArea
	C_LONGINT($0)
	$0:=(This.height)*(This.width)

```

```
// In a project method
C_OBJECT($o)  
C_REAL($area)
$o:=cs.Rectangle.new()  
$area:=$o.getArea(50;100) //5000
```


### Class constructor

#### Syntax

```
// Class: MyClass
Class Constructor
// code
```

A class constructor function, which can accept parameters, can be used to define a user class. 

In that case, when you call the `new()` class member method, the class constructor is called with the parameters optionnally passed to the `new()` function.

For a class constructor function, the `Current method name` command returns: "<ClassName>.constructor", for example "MyClass.constructor".


#### Example:

```
// Class: MyClass
// Class constructor of MyClass
Class Constructor
C_TEXT($1)
This.name:=$1
```

```
// In a project method
// You can instantiate an object
C_OBJECT($o)
$o:=cs.MyClass.new("HelloWorld")  
// $o = {"name":"HelloWorld"}
```




### Class extends \<ClassName>

#### Syntax

```
// Class: ChildClass
Class extends ParentClass
```

The `Class extends` keyword is used in class declaration to create a user class which is a child of another user class. The child class inherits all functions of the parent class.

Class extension must respect the following rules:

- A user class cannot extend a built-in class (except 4D.Object which is extended by default for user classes)
- A user class cannot extend a user class from another database or component.
- A user class cannot extend itself.
- It is not possible to extend classes in a circular way (i.e. "a" extends "b" that extends "a"). 

Breaking such a rule is not detected by the code editor or the interpreter, only the compiler and `check syntax' will throw an error in this case.

An extended class can call the constructor of its parent class using the [`Super`](#super) command.

#### Example

This example creates a class called `Square` from a class called `Polygon`.
 
```4d
  //Class: Square
  //path: Classes/Square.4dm 

 Class extends Polygon
 
 Class constructor
 C_LONGINT($1)
 
  // It calls the parent class's constructor with lengths
  // provided for the Polygon's width and height
Super($1;$1)
  // In derived classes, Super must be called before you
  // can use 'This'
 This.name:="Square"

Function getArea
C_LONGINT($0)
$0:=This.height*This.width
```

## Class commands

Several commands of the 4D language allows you to handle class features.

### Super

#### Syntax

```
Super {( param )} {-> Object} 
```


The `Super` command allows calls to the `superclass`, i.e. the parent class.

- in a [constructor code](#class-constructor), `Super` is a command that allows to call the constructor of the superclass.
- in a [class member function](#class-function), `Super` designates the prototype of the superclass and allows to call a function of the superclass hierarchy.



### OB Class

#### Syntax

```
OB Class ( object ) -> Object | Null
```

`OB Class` returns the class of the object passed in parameter. 


### OB Instance of

#### Syntax

```
OB Instance of ( object ; class ) -> Boolean
```

`OB Instance of` returns `true` if `object` belongs to `class` or to one of its inherited classes, and `false` otherwise.