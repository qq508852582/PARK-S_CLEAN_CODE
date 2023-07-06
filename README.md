# PARK'S_CLEAN_CODE


### Disclaimer

This is a summary/notes of the book "Clean Code - A Handbook of Agile Software Craftsmanship by Robert C. Martin". The below notes are my personal notes and I hope it's not a copyright infringement. If it is, please contact me in order to remove this file from github.


## Clean Code - A Handbook of Agile Software Craftsmanship by Robert C. Martin.

Three important design principles are referenced in this book frequently. They are,

- OCP - Open-Closed Principle - Classes should be open for extension but closed for modification.
- SRP - Single Responsibility Principle - A class should have one, and only one, reason to change.
- DIP - Dependency Inversion Principle - Classes should depend upon abstractions, not on concrete details.

### Chapter:1 Clean Code
1.	Writing clean code requires discipline. We need to work hard to get “code-sense”.
2.	Code without test is not clean.	
4.	Boy Scout Rule tells us we should leave it cleaner than we found it. The clean up does not have to be something big.
5.	To write a new code, we need to read the old one, the ratio of time spent reading vs writing is 10:1.

### Chapter:2 Names
1.	Use intention revealing names. We can improve the code with revealing naming.
2.	Do not use number series naming.
3.	Use Pronounceable names.
4.	Use Searchable names.
5.	Leave interfaces unadorned. ShapeFactory and ShapeFactoryImp are better than IShapeFactory and ShapeFactory
6.	Avoid mental mapping.  Be clear in naming things. One difference between a smart programmer and a professional programmer is that the professional understands clarity is king. Professionals use their powers for good and write code that others can understand.
7.	When constructors are overloaded use static factory methods with names that describe the arguments.

   ``` 
   Complex fulcrumPoint = Complex.FromRealNumber(23.0); 
 
   ```
   
   is better than the below code.
   
   ``` 
    Complex fulcrumPoint = new Complex(23.0); 
   
   ```
   
8.	Avoid using same word for two purposes.
9.	Use problem domain names.

### Chapter:3 Functions
1.	Function should be small and 20 lines length
2.	Function should do one thing, they should do it well and they should do it only.
3.	The code should be read from top to bottom, so arrange the function like that. Place the function right below the function that is calling. 
4. Sometime, its worth to use Polymorphism rather than a switch/case. Using a Switch case violates SRP and OCP principles. Instead, we can create different classes one for each switch and have the switch/case in AbstractFactory. 
5.	Be consistent in the function names.

### Chapter:4 Comments
1.	Always try to explain yourself in code.
2.	Clear and expressive code with fewer comments is far superior to cluttered and complex code with lots of comments.
3.	Create a function that says the same thing as the comment you want to write.
4.	Sometimes its useful to warn other programmers about certain consequences for which you can write comments. 
    May be a test that should not be run. We can still Junit Ignore annotation to do the same. 
    Something that is not thread safe and warn other programmers not to create static instance of that class. Ex. SimpleDateFromat.
5.	Its sometime reasonable to leave TODO comments. But you don’t want your code to be littered with TODOs. So scan through them periodically and eliminate the ones you can.
6.	If you are writing a public API, then you should certainly write good javadocs.
7.	Any comment that forces you to look in another module to know the meaning is wrong.
8.	Journal comments should be completely removed. Long time there was a reason to create and maintain these log entries but they are more clutter to obfuscate the code
9.	Avoid Noise comment and improve the structure of the code.
10.	Don’t use a comment when you can use a function or variable to explain its purpose.
11.	Avoid closing brace comments for deep nested function and shorten the function instead
12.	Delete the code if you don’t need it. Don’t comment it out.
13.	If you must write a comment, then make sure it describes the code it appears near.  Don’t offer system wide information in the context of a local comment. For example, Port number details in a Java Model class)
14.	Don’t add too much historical information in a comment.
15.	If a comment needs its own explanations, then its better to avoid it.

### Chapter: 5 Formatting
1.	You should take care that your code is nicely formatted.
2.	Small files are easier to read and understand
3.	Related code should appear vertically dense.
4.	Declare variables close to their usage.
5.	Instance variables should be declared at the top of the class.
6.	Dependent functions should be vertically close and the caller should be above the callee. If its all possible.  
7.	Keep lines short and the maximum allowable limit is 120
8.	Don’t break indentation

### Chapter:6 Objects and Data structures
1.	We keep our variables private Nobody depends on them. Freedom to change their type or implementation. If you don't need them don't expose the with public getter/setter
2.	Try to not violate the Law of Demeter rule. It states that “talk to friends, not to strangers”. A method “F” of a Class “C” class should call only methods of these.
   - 	Class C methods
   -  An object created by function “F”.
   -  An object passed to function “F”
   -  An object held in an instance variable of class “C”
3.	Avoid the code like below. Its called “Train Wreck”

```
final String outputDir = ctxt.getOptions().getScratchDir().getAbsolutePath();
````
You tell an object to do something and you should not be asking it about its internals. In this example, the outputDir is being used for creating the scratch file. So instead of doing all this, we can let Context object to create the Scratch file.
```
BufferedOutputStream bos = ctxt.createScratchFileStream(classFileName);
 
``` 
### Chapter:7 Exception Handling
1.	Use exception rather than return codes
2.	Use unchecked exception where ever possible. Because adding checked exception to an existing method will trigger lot of change if that method is used in many places.
3.	Provide context such as error and its reason along with exception.
4.	Define Exception Classes in Terms of a Caller’s Needs. If you happen to use third party library and you want to handle error scenario, then its better to create a wrapper class and handle the error in it. 
5.	Define the Normal flow. To handle the special scenario, we can follow SPECIAL CASE PATTERN. You create a class or configure an object to handles a special case for you. When you do the client does not have to deal with exceptional behavior.
6.	Don’t pass NULL and return NULL

### Chapter:8  Boundaries
1.	Try to encapsulate any boundary interface like Map inside a class. Avoid returning /accepting it an argument to Public API’s. If the Map interface changes, then we have to change multiple places. To avoid this, its better to wrap it like below.

```
 public class Sensor {
  private Map sensors = new HashMap();

  public Sensor getById(String id) {
    return (Sensor) sensor.get(id);
  }
}
```

2.	Learning Test instead of experimenting and trying out the new stuff in our production code.  We could write few tests to explore our understanding of third-party API. So that for each third-party release, we can run through our test cases and understand that what has changes recently

3.	Sometimes, you have boundaries in your system that represents things had not been designed yet. But you can create your own interface with fake implementation for testing purpose.

4.	When we use Code that is out of our control, special care must be taken to protect our investment and make sure future changes is not too costly. Wrap them or use an Adapter to convert from our perfect interface to the provided interface.

### Chapter: 10 Unit Tests
1.	One assert per test
2.	Single concept per test
3.	Follow Build-Operate-Check pattern while writing the test. Each of the test is clearly split into three parts.  The first part builds up the test data, The second part operates on that test data, and the third part checks the operation yielded the expected results.
4.	Clean tests follow the below five rules.
F.I.R.S.T 
 F – Tests should be fast. They should run quickly, otherwise you won’t them frequently.
 I – Tests should be independent
 R- Test should be repeatable in every environment. If its not runnable in any other environment, then you will always have an excuse when they fail.
S – The should have a Boolean output. 	Either they pass or fail. It should be self validating. You should not have to manually compare outputs or check log file to see whether the test pass  
 T – Timely. The test should be written in a timely fashion. It should be written before the production code that makes them pass. 

### Chapter: 10 Classes
1.	Classes should be small
2.	The name of the class should describe what responsibility it fulfills.
3.	Classes should not too many responsibilities
4.	The “Single Responsibility Principle” stats that the class should have only one reason to change. They may collaborate with a few other classes to achieve the expected system behavior.
5.	In general, the more variables a method manipulates the more cohesive that method is to its class. A class in which each variable used by each method is maximally cohesive.

### Chapter: 11 Systems

### Chapter: 12: Emergence
According to Kent, a design is “simple” if it follows these rules.
1.	Runs all the test
2.	Contains no duplication
3.	Expresses the intent of the programmer
4.	Minimizes the number of classes and methods
 	
#### Rule 1: Runs all the tests
Writing tests leads to better designs. 
Tight coupling makes it difficult to write tests. When you write more tests then we use principles like DIP and tools like Dependency Injection, Interfaces and abstraction to minimize coupling

#### Rule 2: Refactoring
Once we have tests, then we are empowered to keep our code clean. We will do this by incrementally refactoring the code. 
During this refactoring step, we can increase cohesion, decrease coupling, separate concerns, modularize system concerns, shrink our function and classes, choose better names and so on.

This is where we also apply the final three rules of simple design. They are Eliminate Duplication, Ensure Expressiveness and minimize the number of classes and methods
- Creating a clean system requires the will to eliminate duplication
- Understanding how to achieve reuse in the small is essential
to achieving reuse in the large.
- The TEMPLATE METHOD pattern is useful for removing higher level duplication.
- The code should be expressive. You can express yourself by choosing good names. You can express yourself by keeping your functions and classes small. Small classes and functions are usually easy to name, easy to write and easy to understand.
- By using standard pattern names such as COMMAND or VISITOR in the names of the classes that implements those patterns, you can easily describe your design to other developers

### Chapter: 13 Concurrency
1.	Keep your concurrency-related code separate from other code.
2.	Take data encapsulation to heart. severely limit the access of any data that may be shared
3.	Look for the possibility of using the copy of data and not sharing the data
4.	Become familiar with Java concurrent packages
5.	Keep your synchronized sections as small as possible

### Chapter:14 Successive refinement
1.	Author did not simply write the program from the beginning to the end in its final form
2.	You can’t write clean code in your first attempt itself. Write the workable code, then don’t move on to the next tasks, instead start cleaning
3.	Separate exception/error logic from the main class if they get too big
4.	Bad code is very expensive so always clean as soon as possible

### Chapter: 15 Junit Internal

### Chapter:16 Refactoring SerialDate
                
### Chapter: 17 Smells

#### Comments
1.	Inappropriate Information
  Its inappropriate for a comment to hold the information better held in a different kind of system such as Git. Comments should be reserved for technical notes and design.
2.	Obsolete Comment
  If you find an obsolete comment, it is best to update it or get rid of it as quickly as possible.
3.	Redundant Comment
  Comments should say things that the code cannot say for itself. Otherwise remove it.
4.	Poorly written comment
5.	Commented out code

#### Environment
1.	Build Requires More Than One Step: 
    Building a project should be a trivial operation. You should be able to check out the system/project with one simple command and then issue other simple command to build it.
2.	Tests Require More Than One Step: 
   You should be able to run all the unit tests with just one command
   
#### Functions
1.	Too many arguments: No argument is best, followed by one, two and three. More than three is questionable. 
2.	Output arguments. In general output arguments should be avoided. If your function must change the state of something, have it change the state of its owning object.
3.	Flag arguments are ugly. Passing a Boolean into a function is a truly terrible practice. In such case, we can split the function into two and call them separately, instead of passing down a Boolean flag.
4.	Dead function – Method that never be called should be removed.

#### General
1.	The source file should contain only one language
2.	Obvious Behavior is unimplemented
Any function or class should implement the behavior that another programmer could reasonably expect.  For example, consider a function that translates the name of day to an Enum that represents that day.

``` Day day = DayDate.StringToDay(String dayName); ```

We would expect that the string “Monday” to be translated as Day.MONDAY. When this behavior is not implemented, then the user or reader of the code can no longer depend on their intuition about function names. They read the code to know about its details.

3.	Incorrect Behavior at the Boundaries
Don’t rely on your intuition. Look for every boundary condition and write a test for it.
4.	Overridden Safeties
Turning off certain compiler warnings (or all warnings) may help you get the build to succeed, but at the risk of endless debugging sessions. Turning off failing tests and telling yourself you’ll get them to pass later is as bad as pretending your credit cards are free money.
5.	Duplication
Find and eliminate duplication wherever you can. The use of Template method/Strategy pattern will help to eliminate duplication when two or modules share same algorithm
6.	Code at Wrong Level of Abstraction
Its important to create abstractions that separate higher level general concepts from lower level detailed concepts. For example, Constants, variables that pertain to the detailed implementation should not be present in the base class. 
7.	Base Classes Depending on Their Derivatives
Base class should know nothing about derived class.  In general, the base and derived classes can be deployed independently in a separate JAR files as a separate component. So, if there is any change in derived class should not have any impact to Base class and the base class component does not need to be redeployed. This means the impact of the change is less. 
8.	Too much information
Hide your data. Hide your utility functions. Hide your constants and your temporaries.
Don’t create classes with lots of methods or lots of instance variables. Don’t create lots of
protected variables and functions for your subclasses. Concentrate on keeping interfaces
very tight and very small. Help keep coupling low by limiting information.
9.	Dead code
10. Vertical Separation
Variable and function should be defined close to where they are used.
11. Inconsistency
 If you do something a certain way, do all similar things in the same way. This goes back
to the principle of least surprise. Be careful with the conventions you choose, and once chosen, be careful to continue to follow them.
12. Clutter
Keep your files well organized, clean and free of clutter. Remove redundant code and unused variables, functions
13. Artificial Coupling
In general an artificial coupling is a coupling between two modules that serves no direct purpose. It is a result of putting a variable, constant, or function in a temporarily convenient, though inappropriate, location. This is lazy and careless.
Take the time to figure out where functions, constants, and variables ought to be
declared. Don’t just toss them in the most convenient place at hand and then leave them there.
14. Feature Envy
The methods of a class should be interested in the variables and functions of the class they belong to, and not the variables and functions of other classes.  If it does, then its better to move that method to other class so that it will have direct access to other class fields. For example, calculating the weekly pay of an employee in a “HourlyPayCalculator” class instead of in HourlyEmployee class.
But sometime, we may need to do that because that method should not be moved to other class as it violates object-oriented design. For example, getting the employee report hours report information. If there is any change in report format, then that requires changes in HourlyEmployee and it violates many principles. 
15. Selector Arguments
In general, it is better to have many functions than to pass some code into a function to select the behavior. The passed arguments could be Boolean, enum or any integer value. 
16. Obscured Intent
Intent the code so that it could easily visible to others.
17. Misplaced Responsibility 
Code should be placed where a reader would naturally expect it to be. For example, calculating the total hours of an employee worked could be in Report module or it could be in Timecard module 
18. Inappropriate Static
 In general, you should prefer nonstatic methods to static methods. When in doubt,
make the function nonstatic. If you really want a function to be static, make sure that there
is no chance that you’ll want it to behave polymorphically
19. Use Explanatory Variables
20. Function Names Should Say What They Do
21. Understand the Algorithm
Before you consider yourself to be done with a function, make sure you understand
how it works. It is not good enough that it passes all the tests. You must know10 that the
solution is correct.
22. Make logical dependencies Physical
 If one module depends upon another, that dependency should be physical, not just logical.
The dependent module should not make assumptions (in other words, logical dependencies)
about the module it depends upon. Rather it should explicitly ask that module for all
the information it depends upon
23. Prefer Polymorphism to If/Else or Switch/Case
24. Follow standard conventions
25. Replace magic number with Named constants
26. Be precise
When you make a decision in your code, make sure you make it precisely. Know why
you have made it and how you will deal with any exceptions
27. Structure over convention
Enforce design decisions with structure over convention. Naming conventions are good,
but they are inferior to structures that force compliance. For example, switch/cases with
nicely named enumerations are inferior to base classes with abstract methods
28. Encapsulate conditional
Extract functions that explain the intent of the conditional
29. Avoid negative conditionals
30. Function should do one thing
31. Hidden temporal couplings
Temporal couplings are often necessary, but you should not hide the coupling. We can do this by creating a bucket brigade. Each function produces a result that the next function needs, so there is no reasonable way to call them out of order
32. Don’t be arbitrary.
If you see any public static inner class that belongs to another class but used by other classes as well. Then its better to move that class to a top level class. 
33. Encapsulate Boundary conditions
Boundary conditions are hard to keep track of. Put the processing for them in one place.
Don’t let them leak all over the code. We don’t want swarms of +1s and -1s scattered hither
and yon.
34. Functions Should Descend Only One Level of Abstraction
35. Keep Configurable Data at High Levels
If you have a constant such as a default or configuration value that is known and expected
at a high level of abstraction, do not bury it in a low-level function. Expose it as an argument
to that low-level function called from the high-level function. Example, Having a constants file that contains all the default configuration details
36. Avoid Transitive Navigation
In general we don’t want a single module to know much about its collaborators. More specifically,
if A collaborates with B, and B collaborates with C, we don’t want modules that use
A to know about C. (For example, we don’t want a.getB().getC().doSomething())

#### Java
1.	Avoid long import Lists by using wildcards
2.	Don’t inherit constants. Avoid adding constants in an interface and implements it in a class for using constants. Use a static import instead.
3.	Use Enums over constants
##### Names
1.	Choose descriptive names
2.	Choose names at the appropriate level of abstraction
3.	Use standard nomenclature wherever its possible
4.	Use long names for long scopes
5.	Avoid encodings
6.	Name should describe side-effects. Function name should describe everything that a function does. Don’t hide the side effects with a name. Refer the below example. The better name would be “createOrReturnOos”.
```
public ObjectOutputStream getOos() throws IOException {
   if (m_oos == null) {
     m_oos = new ObjectOutputStream(m_socket.getOutputStream());
  }
  return m_oos;
}
```
#### Tests
1.	Insufficient tests
2.	Use a coverage tool
3.	Don’t skip Trivial Tests
4.	An Ignored Test Is a Question about an Ambiguity
5.	Test Boundary conditions
6.	Exhaustively Test Near Bugs
7.	Patterns of Failure Are Revealing
8.	Test Coverage Patterns Can Be Revealing
9.	Tests should run fast
