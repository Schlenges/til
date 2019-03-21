# Java 8 Features

## Lambda
Lambda expressions are used primarily to define inline implementation of a functional interface, i.e., an interface with a single method only. 
Lambda expression eliminates the need of anonymous class and gives a very simple yet powerful functional programming capability to Java.

```Java
public class Java8Tester {

   public static void main(String args[]) {
      Java8Tester tester = new Java8Tester();
		
      //with type declaration
      MathOperation addition = (int a, int b) -> a + b;
		
      //with out type declaration
      MathOperation subtraction = (a, b) -> a - b;
		
      //with return statement along with curly braces
      MathOperation multiplication = (int a, int b) -> { return a * b; };
		
      //without return statement and without curly braces
      MathOperation division = (int a, int b) -> a / b;
		
      System.out.println("10 + 5 = " + tester.operate(10, 5, addition));
      System.out.println("10 - 5 = " + tester.operate(10, 5, subtraction));
      System.out.println("10 x 5 = " + tester.operate(10, 5, multiplication));
      System.out.println("10 / 5 = " + tester.operate(10, 5, division));
		
      //without parenthesis
      GreetingService greetService1 = message ->
      System.out.println("Hello " + message);
		
      //with parenthesis
      GreetingService greetService2 = (message) ->
      System.out.println("Hello " + message);
		
      greetService1.sayMessage("Mahesh");
      greetService2.sayMessage("Suresh");
   }
	
   interface MathOperation {
      int operation(int a, int b);
   }
	
   interface GreetingService {
      void sayMessage(String message);
   }
	
   private int operate(int a, int b, MathOperation mathOperation) {
      return mathOperation.operation(a, b);
   }
}
```

Using lambda expression, you can refer to any final variable or effectively final variable (which is assigned only once). Lambda expression throws a compilation error, if a variable is assigned a value the second time.

```Java
public class Java8Tester {

   final static String salutation = "Hello! ";
   
   public static void main(String args[]) {
      GreetingService greetService1 = message -> 
      System.out.println(salutation + message);
      greetService1.sayMessage("Mahesh");
   }
	
   interface GreetingService {
      void sayMessage(String message);
   }
}
```

## Method reference
Method references help to point to methods by their names. A method reference is described using "::" symbol. A method reference can be used to point the following types of methods:
- Static methods
- Instance methods
- Constructors using new operator (TreeSet::new)

Here we pass the System.out::println method as a static method reference:

```Java
import java.util.List;
import java.util.ArrayList;

public class Java8Tester {

   public static void main(String args[]) {
      // List<String> names = new ArrayList();
      List names = new ArrayList();
		
      names.add("Mahesh");
      names.add("Suresh");
      names.add("Ramesh");
      names.add("Naresh");
      names.add("Kalpesh");

      /* for(String name : names){
        System.out.println(name);
      } */

      /* Using Lambda:
        names.forEach(name -> System.out.println(name));
       */
    
      // Using Method Reference
      names.forEach(System.out::println);
   }
}
```

## Functional Interfaces
Functional interfaces have a single functionality to exhibit. For example, a Comparable interface with a single method ‘compareTo’ is used for comparison purpose. Java 8 has defined a lot of functional interfaces to be used extensively in lambda expressions, defined in the java.util.Function package.

**Example with the Predicate <T> interface**
Predicate <T> interface is a functional interface with a method test(Object) to return a Boolean value. This interface signifies that an object is tested to be true or false.

```Java
import java.util.Arrays;
import java.util.List;
import java.util.function.Predicate;

public class Java8Tester {

   public static void main(String args[]) {
      List<Integer> list = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9);
		
      // Predicate<Integer> predicate = n -> true
      // n is passed as parameter to test method of Predicate interface
      // test method will always return true no matter what value n has.
		
      System.out.println("Print all numbers:");
		
      //pass n as parameter
      eval(list, n->true);
		
      // Predicate<Integer> predicate1 = n -> n%2 == 0
      // n is passed as parameter to test method of Predicate interface
      // test method will return true if n%2 comes to be zero
		
      System.out.println("Print even numbers:");
      eval(list, n-> n%2 == 0 );
		
      // Predicate<Integer> predicate2 = n -> n > 3
      // n is passed as parameter to test method of Predicate interface
      // test method will return true if n is greater than 3.
		
      System.out.println("Print numbers greater than 3:");
      eval(list, n-> n > 3 );
   }
	
   public static void eval(List<Integer> list, Predicate<Integer> predicate) {

      for(Integer n: list) {

         if(predicate.test(n)) {
            System.out.println(n + " ");
         }
      }
   }
}
```

## Streams API

A Stream represents a sequence of objects from a source, which supports aggregate operations. Following are the characteristics of a Stream:

- **Sequence of elements** − A stream provides a set of elements of specific type in a sequential manner. A stream gets/computes elements on demand. It never stores the elements.

- **Source** − A Stream takes Collections, Arrays, or I/O resources as input source.

- **Aggregate operations** − Streams support aggregate operations like filter, map, limit, reduce, find, match, and so on.

- **Pipelining** − Most of the stream operations return a stream themselves so that their result can be pipelined. These operations are called *intermediate operations* and their function is to take input, process it, and return output to the target. The collect() method is a terminal operation which is normally present at the end of the pipelining operation to mark the end of the stream.

- **Automatic iterations** − Stream operations do the iterations internally over the source elements provided, in contrast to Collections, where explicit iteration is required.

With Java 8, the Collection interface has two methods to generate a Stream: ```stream()``` and ```parallelStream()```.

### Operations

**forEach()**
```Java
Random random = new Random();
random.ints().limit(10).forEach(System.out::println); // ints() returns a stream
```  

**map()**  
Used to map each element to its corresponding result.
```Java
List<Integer> numbers = Arrays.asList(3, 2, 2, 3, 7, 3, 5);

//get list of unique squares
List<Integer> squaresList = numbers.stream().map( i -> i*i).distinct().collect(Collectors.toList());
```  

**filter()**  
Used to filter elements based on a specific criteria.
```Java
List<String> strings = Arrays.asList("abc", "", "bc", "efg", "abcd","", "jkl");

// filter out all empty strings
List<String> filtered = strings.stream().filter(string -> !string.isEmpty()).collect(Collectors.toList());
```

**limit()**  
Used to reduce the size of the stream.

**sorted()**
Used to sort the stream. You can provide your own Comparator for the desired sort order.

```Java
Random random = new Random();
random.ints().limit(10).sorted().forEach(System.out::println);
```

### Collectors
Collectors are used to 'collect' the result of the stream processing. They can be used to return a list or a string.

```Java
List<String> strings = Arrays.asList("abc", "", "bc", "efg", "abcd","", "jkl");

List<String> filtered = strings.stream().filter(string -> !string.isEmpty()).collect(Collectors.toList());
System.out.println("Filtered List: " + filtered);

String mergedString = strings.stream().filter(string -> !string.isEmpty()).collect(Collectors.joining(", "));
System.out.println("Merged String: " + mergedString);
```