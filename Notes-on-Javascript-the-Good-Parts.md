##Chapter 1

> Most programming languages contain good parts and bad parts. I discovered that I could be a better programmer by using only the good parts and avoiding the bad parts. After all, how can you build something good out of bad parts?

The best parts of Javascript include:
* functions
* loose typing (variables are declared as variables, without a type)
* dynamic objects
* object literal notation (where you can create an object already with a list of key/value pairs inside curly braces)

The worst parts include global variables - there is a common _global object_ namespace where they're all lumped together and they're essential to the language.

Javascript has a _class free_ object makeup, relying instead on objects inheriting properties directly from other objects - this is **prototypal inheritance**.

<a name="Chapter2"/>
##Chapter 2

Always use // for comments, even multi-line ones to avoid having to escape `/*` characters.

###Numbers

* There is a single, 64-bit floating point number type.
* _NaN_ (Not-a-Number) is not equal to any value (including itself) and is essentially an **illegal number value**.
* Use _isNan(number)_ to check for NaNs.

Number methods are discussed in [Chapter 8](#Chapter8).

###Strings

* 16-bit character set and don't have character tpes.
* Backslashes (\\\) are used for **escaping characters** that could cause problems in strings.
* Strings are **immutable**.

String methods are discussed in [Chapter 8](#Chapter8).

###Statements

* _Inside_ a function, the var statement creates variables local to that function
* _switch, while, for_ and _do_ statements can have an optional **label*  which can be used with `break` and `continue` to provide more precise [control over exactly which statement to break or continue](http://www.tutorialspoint.com/cgi-bin/practice.cgi?file=javascript_19). Format: `continue labelname;`


<a name="Chapter8"/>
##Chapter 8

##Appendix A - the Awful Parts

###Global variables
These are variables that are visible throughout the code in any scope. They can be **changed at any time** by any part of the program which makes them **unreliable in larger complex programs**. This can also lead to naming conflicts which can cause your code to fail or you to accidentally overwrite a global variable.