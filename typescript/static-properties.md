9

While other languages like C# do have static constructors, TypeScript (and JavaScript) do not have this feature. That said, you can still define a function statically which you invoke yourself. First, let me explain in which cases you might need a static constructor before I go over the options you have.

When to use a static constructor

A static constructor is useful in cases in which you need to calculate the value of a static attribute. If you just want to set an attribute (to a known value), you don't need a static constructor. It can be done like this:
``` typescript
class Example {
    public static info = 123;
    // ...
}
```
In case you need to compute the value, you have three ways to "simulate" a static constructor. I go over the options below and set the static attribute "info" in each sample.

Option 1: Call an initialize function after the class is declared
In the code below, the function _initialize is statically defined in the class and invoked immediately after the class is declared. Inside the function, this refers to the class, meaning that the keyword can be used to set any static values (like info in the sample below). Note, that it is not possible to make the function private, as it is invoked from outside.
``` typescript
class Example {
    public static info: number;

    public static _initialize() {
        // ...
        this.info = 123;
    }
}
```
Example._initialize();
Option 2: Directly invoke the function inside the class
The second option is to use a function, which is directly called after its creation inside the class. The function only looks like its part of the class, but has no relation to the class itself (except being defined inside of it), meaning that you cannot use this inside the function.
``` typescript
class Example {
    static info: number;

    private static _initialize = (() => {
        // "this" cannot be used here
        Example.info = 1234;
    })();
}
```
Alternative: Calculate a single attribute
Based on the same idea as option 2, you can calculate a single attribute by returning the value. This might be handy in cases where you only want to calculate one attribute of the class.
``` typescript
class Example {
    public static info = (() => {
        // ... calculate the value and return it
        return 123;
    })();
}
```
What to use
If you only want to calculate a single static attribute, you might want to use the last (alternative) approach. In case you need to do more complex calculations or want to set more attributes, I recommend using option 1 if you don't mind the function being public. Otherwise, go with option 2.

Note, that there is an issue in the TypeScript repository, that contains some more discussions regarding option 1 and 2.

### References
https://stackoverflow.com/questions/49589518/static-constructor-typescript
