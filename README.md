# BGI-VerySimple
33 BGI-VerySimple/VerySimple.java 64 34 BGI-VerySimple/Main.java 64

## 8.3 Access to classes and their members  

Generally, fields of a class should be somehow protected from unrestricted access from other classes (we call it hermetization, or encapsulation). They should be manipulated only by methods, which can ensure that they are operated upon in a safe and consistent way. We have control over accessibility of members by adding, at the beginning of their declaration, an appropriate keyword: private, protected, public or, not specifying any of these, we get the default. They have the following meaning:  

* default (or package private) — no special keyword — the field can be accessed from functions (static or non-static) of the same class and also from functions of all classes belonging to the same package;  
* private — the field can be accessed only from functions of the same class;  

* protected — the field can be accessed from functions of the same class, form classes in the same package, and also from classes which extend this class (inherit from it) (Sec. 10, p. 87) even if they belong to a different package;  

* public — the field can be accessed from functions of all classes where our class is visible.  

The same rules apply to all members: also constructors and functions. It is recommended to declare all fields (data members) as private (or protected).  

But then a problem arises: if we do not provide public methods which modify the fields, it will never be possible to assign any useful value to them! Of course, there is a way to do it — by defining constructors (see the next section).

We can also declare whole classes as public or having default accessibility. If a class is declared with the default accessibility, its name will be visible only in other classes, but only those from the same package. There also exist the so called inner classes, i.e., classes defined inside definition of other classes, which will be covered later. Such classes may also be declared as private or protected.

The ‘entry’ class, the one which contains main, must always be declared as public (and the main function itself must also be public).

## 8.4 Constructors and methods   

So let us make the fields of a class private. For example:

## Listing 33 BGI-VerySimple/VerySimple.java  

```java
// BGI-VerySimple/VerySimple.java
 
public class VerySimple {
    private int    age;
    private String name;

      // constructor
    public VerySimple(int age, String n) {
        this.age = age;
        name = n;
    }
      //getter
    public int getAge()  {
        return age;
    }
      // setter
    void setAge(int a) { // package private
        age = a;
    }
      // getter (with no corresponding setter)
    public String getName() {
        return name;
    }
}
```

and in Main we can create objects of this class, and even modify them (because both constructor and setAge are not private or protected. Of course, in our main function, which is a function of another class, we cannot directly access the fields of class `VerySimple`, but we can use public constructors and methods, which, being members of the class `VerySimple`, do have access to all members of all objects of this class. In particular, constructor is a very special function:  

* its name must be the same as the name of the class;

* it does not declare any return type (even void);  

* it will be automatically invoked only at the very end of the process of constructing an object — there is no way to invoke it on an object which has already been created before.  

Normally, constructor are used to initialize fields of the object being created, but generally they can do whatever we wish.

```java
// BGI-VerySimple/Main.java
 
public class Main {
    public static void main(String[] args) {
        VerySimple alice = new VerySimple(23,"Alice");
        VerySimple bob   = new VerySimple(21,"Bob");
        alice.setAge(18);

        System.out.println(
                alice.getName()  + " " + alice.getAge());
        System.out.println(
                bob.getName()    + " " + bob.getAge());
    }
}
```

To use a specific constructor during creation of an object, we just write, after the name
of the class, arguments — as many of them as expected by the constructor, and of
appropriate types. The program, as the previous one, prints  

```
Alice 18
Bob 21 
```

In a class, one can define many methods or constructors with the same name (in case
of constructors there is no way to avoid it, as all constructors have to be named as the
class they are defined in). This is called overloading.  

However, overloaded functions must differ in number of parameters and/or these parameters’ types, so when they
are invoked, the compiler knows (by looking at arguments) which one is meant. The
precise rules used by the compiler to resolve which method or constructor should be
used in a given context are rather complicated. However, there should be no problem
if the differences are sufficiently obvious (different number of parameters, difference in
types like between String and int etc.).

If we don’t define any constructor, the compiler will add one — parameterless and doing
nothing. Then all fields will be initialized with their default values: zero for numeric
types, null for references, false for booleans. A parameterless constructor, whether
it is created by the compiler or defined by ourselves, is called default constructor.
However, the compiler will not create any default constructor if there is at least one
constructor in a class defined by us.

When a method is invoked, it must always be invoked on an object (in the example,
it was the object pointed to by reference alice). As we have already mentioned, the
reference (address) to this object is passed to the function as an additional, ‘hidden’
argument. As it is not mentioned on the parameter list of the function, its name is
once for all fixed: this. Inside the function, when we refer to a name (e.g., age) and
there is no local variable with this name, then we can omit the reference to object of
which it is a member: it is understood that it is this.age that is meant.
