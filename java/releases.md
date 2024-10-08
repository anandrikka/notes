---
description: Go through all releases and make important notes for each release.
---

# New Features

### Version 8

| Change | Explanation |
| ------ | ----------- |
|        |             |
|        |             |
|        |             |

Java 8 is a major improvement from v7. It included a lot of features to cope with other programming languages. It's a step towards functional programming and introduces _<mark style="background-color:blue;">Functions</mark>_ as first-class citizens in Java. The focus has been really on reducing the boilerplate code to write the stuff.

### Functions or Lambda Expressions

Before v8 we could not pass a method as a reference it had to be either a primitive value or object reference. Now methods are first-class citizens and we can pass methods as reference values.

A _**Function or Lambda**_ is different from a _**Method.**_ A method is typically associated with a class, but the function is anonymous, has no name, and is not associated with any class similar to the method it can accept parameters, have a return type, and throw errors.

A Functional interface is an interface with a single method. We have already seen a lot of functional interfaces before like `Runnable`

```java
interface Runnable {
    public void run();
}

Runnable r = new Runnable() {
    public void run() {
        System.out.println("Hello World!")
    }
}
```

With lambda, we can write the shorthand notion:

```java
Runnable r = () -> System.out.println("Hello World"); // This is a function
```

> A Lamda is simply a shorthand notation of an anonymous class with a single method in it. It is a clear and concise way to represent a method inside a functional interface using an expression&#x20;

Valid forms of lambda expressions are:

```java
// returns void
() -> {}
// Single line
() -> System.out.print("Hello World");
// Multiline
() -> {
    System.out.println("Hello World");
    return 10;
}
```

#### Function descriptor

The signature of the abstract method of the functional interface describes the signature of the lambda expressions. We call this as `Function descriptor`

For example: `() -> void` can be associated with any functional interface that returns `void` It can be a Runnable interface.

| Functional Interface  | Function Descriptor | Abstract Method |
| --------------------- | ------------------- | --------------- |
| `Predicate<T>`        | `T -> boolean`      | `test`          |
| `Consumer<T>`         | `T -> void`         | `accept`        |
| `Function<T, R>`      | `T -> R`            | `apply`         |
| `Supplier<T>`         | `() -> T`           | `get`           |
| `BiFunction<T, R, V>` | `(T, R) -> V`       | `apply`         |
| `UnaryOperator<T>`    | `T -> T`            | `identity`      |
| `BinaryOperator<T>`   | `(T, T) -> T`       | `apply`         |
| `BiConsumer<T, R>`    | `(T, R) -> void`    | `accept`        |

`UnaryOperator, BinaryOperator` are special kind of `Function` with only a single `type`

> In java, there is a cost to `boxing` and `unboxing`as boxed values ise more memory and require addtional memory lookups to fetch the wrapped primitive values

```java
Predicate<Integer> checkForEven = (a) -> a % 2 == 0;
checkForEven.test(1000); // Boxes the value 1000 to an integer object

IntPredicate<Integer> checkForOdd = (a) -> a % 2 != 0;
checkForOdd.test(1001); // Value is not boxed
```

Java provides `Int, Double, Long and Boolean` functional interfaces to avoid boxing and unboxing, so when necessary use them to increase performance.

### Method References

Java allows to pass methods inside the class as lambdas if they match the function descriptor

There are three ways a method can be used as lambda (Method reference)

1. A method reference to a static method inside a class.

```java
(args) -> ClassName.staticMethod(args);
// ClassName::staticMethod
```

2. A method reference to an instance method of an arbitrary object of a particular type

```java
(obj, args) -> obj.instanceMethod(args);
// ClassName::instanceMethod (`obj` className)
```

3. A method reference to a local instance variable method used inside lambda

```java
(args) -> expr.instanceMethod(args);
// expr::instanceMethod
```

{% code overflow="wrap" lineNumbers="true" fullWidth="false" %}
```java
class Person {
    int age;
    String name;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return this.name;
    }

    public int getAge() {
        return this.age;
    }

    // static method accessed as Person.compareByName
    public static int compareByName(Person p1, Person p2) {
        return p1.getName().compareTo(p2.getName());
    }
    // static method accessed as Person.compareByAge
    public static int compareByAge(Person p1, Person p2) {
        return p1.getAge() - p2.getAge();
    }
}

class PersonUtils {
    public int getLengthOfPerson(Person p) {
        return p.getName().length();
    }
}

public class TestMethodReferences {
    // Sort People By Name
    public static void staticMethodReference(List<Person> people) {
        // Before Java 8
        Collections.sort(people, new Comparator<Person>() {
            @Override
            public int compare(Person o1, Person o2) {
                return o1.getName().compareTo(o2.getName());
            }
        });

        // Using lambda
        Collections.sort(people, (p1, p2) -> p1.getName().compareTo(p2.getName()));

        // Using static method on the the class
        Collections.sort(people, (p1, p2) -> Person.compareByName(p1, p2));
        
        // Using method reference, here arguments received by lambda are passed into the static method
        Collections.sort(people, Person::compareByName);
    }
    
    // Get the names of all people
    public static void objectTypeInstanceMethodReference(List<Person> people) {
        // Using lambda
        people.stream().map((p) -> p.getName());
        
        // Using method reference on the object type in lambda expression
        people.stream().map(Person::getName);
    }
    
    // Get names by local variable reference
    public static void localVariableInstanceMethodReference(List<Person> people) {
        PersonUtils personUtils = new PersonUtils();
        
        // Using lambda
        people.stream().map((p) -> personUtils.getLengthOfPerson(p));
        
        // Using local variable instance method reference
        people.stream().map(personUtils::getLengthOfPerson);
    }
}

```
{% endcode %}

#### Composing Lambdas

We can chain `Predicate` and `Function` lambdas, thus helping us to handle more complex conditions with ease.

Predicate has three default methods `and`, `or`, and `negate`

<pre class="language-java" data-overflow="wrap"><code class="lang-java">Predicate&#x3C;Person> male = (p) -> "male".equals(p.getGender());

// Not male
Predicate&#x3C;Person> notMale = male.negate();
// In Java 11
Predicate.not(male);

// Male and age > 50
Predicate&#x3C;P> maleAndAgeAbove50 = male.and((p) -> p.getAge() > 50);

<strong>// Male and age > 40 or female and age &#x3C; 20
</strong>((p) -> "male".equals(p.getGender()).and((p) -> p.getAge() > 40)).or(male.negate().and((p) -> p.getAge() &#x3C; 20))
</code></pre>

Function has 2 default methods `andThen` and `compose`

```java
Function<Integer, Integer> f = x -> x + 1
Function<Integer, Integer> g = x -> x * 2

// andThen
Function<Integer, Integer> h = f.andThen(g);
h.apply(1);
// f(1) = 1 + 1 = 2
// g(f(1)) = 2 * 2 = 4

// compose
Function<Integer, Integer> h = f.compose(g);
h.apply(1);
// g(1) = 1 * 2 = 2
// f(g(1)) = 2 + 1 = 3
```

### Streams

A Stream is a sequence of elements from a collection source that support data processing operations.&#x20;

Streams hide the internal implementation and help us write the code declaratively.

<mark style="background-color:red;">One of the main advantages of functional-style programming over an imperative approach, is I just need to tell what I need, and not the steps involved to obtain it.</mark>

Streams can be `chained` and do a series of operations before performing one final termination operator.

#### Intermediate Operators:

<table><thead><tr><th width="278">Operation</th><th>Type / Functional Interface</th></tr></thead><tbody><tr><td><code>filter</code></td><td><code>Predicate&#x3C;T></code></td></tr><tr><td><code>distinct</code></td><td></td></tr><tr><td><code>skip</code></td><td><code>long</code></td></tr><tr><td><code>limit</code></td><td><code>long</code></td></tr><tr><td><code>map</code></td><td><code>Function&#x3C;T, R></code></td></tr><tr><td><code>flatMap</code></td><td><code>Function&#x3C;T, Stream&#x3C;R>></code></td></tr><tr><td><code>sorted</code></td><td><code>Comparator&#x3C;T></code> </td></tr></tbody></table>

#### Terminal Operators:

<table><thead><tr><th width="281">Operation</th><th>Type / Functional Interface</th></tr></thead><tbody><tr><td><code>anyMatch</code></td><td><code>Predicate&#x3C;T></code></td></tr><tr><td><code>noneMatch</code></td><td><code>Predicate&#x3C;T></code></td></tr><tr><td><code>allMatch</code></td><td><code>Predicate&#x3C;T></code></td></tr><tr><td><code>findAny</code></td><td></td></tr><tr><td><code>findFirst</code></td><td></td></tr><tr><td><code>forEach</code></td><td><code>Consumer&#x3C;T></code></td></tr><tr><td><code>collect</code></td><td><code>Collector&#x3C;T, A, R></code></td></tr><tr><td><code>reduce</code></td><td><code>BinaryOperator&#x3C;T></code></td></tr><tr><td><code>count</code></td><td></td></tr></tbody></table>

{% tabs %}
{% tab title="Trader.java" %}
<pre class="language-java"><code class="lang-java"><strong>public class Trader {
</strong>
    private String name;
    private String city;

    public Trader(String n, String c){
        this.name = n;
        this.city = c;
    }

    public String getName(){
        return this.name;
    }

    public String getCity(){
        return this.city;
    }

    public void setCity(String newCity){
        this.city = newCity;
    }

    public String toString(){
        return "Trader:"+this.name + " in " + this.city;
    }
}

</code></pre>
{% endtab %}

{% tab title="Transaction.java" %}
```java
public class Transaction {

    private Trader trader;
    private int year;
    private int value;

    public Transaction(Trader trader, int year, int value)
    {
        this.trader = trader;
        this.year = year;
        this.value = value;
    }

    public Trader getTrader(){
        return this.trader;
    }

    public int getYear(){
        return this.year;
    }

    public int getValue(){
        return this.value;
    }

    public String toString(){
        return "{" + this.trader + ", " +
                "year: "+this.year+", " +
                "value:" + this.value +"}";
    }
}
```
{% endtab %}

{% tab title="TestStreams.java" %}
{% code overflow="wrap" %}
```java
import java.util.Arrays;
import java.util.Comparator;
import java.util.List;
import java.util.Optional;
import java.util.stream.Collectors;

public class TestStreams {
    public static void main(String[] args) {
        Trader raoul = new Trader("Raoul", "Cambridge");
        Trader mario = new Trader("Mario","Milan");
        Trader alan = new Trader("Alan","Cambridge");
        Trader brian = new Trader("Brian","Cambridge");

        List<Transaction> transactions = Arrays.asList(
                new Transaction(brian, 2011, 300),
                new Transaction(raoul, 2012, 1000),
                new Transaction(raoul, 2011, 400),
                new Transaction(mario, 2012, 710),
                new Transaction(mario, 2012, 700),
                new Transaction(alan, 2012, 950)
        );

        // Query 1: Find all transactions from year 2011 and sort them by value (small to high).
        transactions.stream().filter((t) -> 2011 == t.getYear()).sorted((t1, t2) -> t1.getValue() - t2.getValue());
        List<Transaction> tr2011 = transactions
                .stream()
                .filter((t) -> 2011 == t.getYear())
                .sorted(Comparator.comparing(Transaction::getValue))
                .collect(Collectors.toList());
        System.out.println(tr2011);

        // Query 2: What are all the unique cities where the traders work?
        List<String> cities = transactions
                .stream()
                .map((t) -> t.getTrader().getCity())
                .distinct()
                .collect(Collectors.toList());
        System.out.println(cities);

        // Query 3: Find all traders from Cambridge and sort them by name.
        List<Trader> traderNames = transactions.stream()
                .map(Transaction::getTrader)
                .filter((t) -> "Cambridge".equals(t.getCity()))
                .distinct()
                .sorted(Comparator.comparing(Trader::getName))
                .collect(Collectors.toList());
        System.out.println(traderNames);

        // Query 4: Return a string of all traders’ names sorted alphabetically.
        String traderNames2 = transactions
                .stream()
                .map(Transaction::getTrader)
                .distinct()
                .sorted(Comparator.comparing(Trader::getName))
                .map(Trader::getName)
                .reduce("", (n1, n2) -> n1 + n2);
        System.out.println(traderNames2);

        // Query 5: Are there any trader based in Milan?
        boolean milanBased = transactions.stream().anyMatch((t) -> t.getTrader().getCity().equals("Milan"));
        System.out.println(milanBased);

        // Query 6: Update all transactions so that the traders from Milan are set to Cambridge.
        transactions.stream().map(Transaction::getTrader).filter((t) -> "Milan".equals(t.getCity())).forEach((t) -> t.setCity("Cambridge"));
        System.out.println(transactions);

        // Query 7: What's the highest value in all the transactions?
        int maxValue = transactions.stream().map(Transaction::getValue).reduce(0, Integer::max);
        System.out.println(maxValue);

    }
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

#### Building Streams:

We can explicitly create streams from values (`Stream.of`), arrays (`Arrays.stream`), and files.

Other than that we have 2 infinite streams

`Stream.iterate` and `Stream.generate`

Generate generates values based on the `Supplier` passed to the `generate` function.

### Collectors

`collect` on streams is a complex `reduce` operator under the hood. `Collectors` are `functional` in style and all the implementation logic is hidden under the hood.

While we can write our own `collectors`. Java has a lot of built-in `collector` utils for the most common use cases. They are all derived from a basic `reducing` factory collector method.

`import static java.utils.streams.Collectors.*;`

<table data-full-width="true"><thead><tr><th width="216">Collector</th><th width="169">Return Type</th><th width="240">Summary</th><th>Example</th></tr></thead><tbody><tr><td><code>toList</code></td><td><code>List&#x3C;T></code></td><td>Gather all stream elements into list.</td><td><code>.collect(toList())</code></td></tr><tr><td><code>toSet</code></td><td><code>Set&#x3C;T></code></td><td>Gathers all unique elements eliminating duplicates</td><td><code>.collect(toSet())</code></td></tr><tr><td><code>toCollection</code></td><td><code>Collection&#x3C;T></code></td><td>We can provide the supplier with the collection type</td><td><code>.collect(toCollection(), ArrayList::new)</code></td></tr><tr><td><code>counting</code></td><td><code>Long</code></td><td>Returns the count of elements</td><td><code>.collect(counting())</code></td></tr><tr><td><code>summingInt</code></td><td><code>Integer</code></td><td>A sum of an integer property of the items in the stream. We also have <code>summingLong</code>, <code>summingDouble</code></td><td><code>.collect(summingInt(Dish::getCalories))</code></td></tr><tr><td><code>averagingInt</code></td><td><code>Double</code></td><td>Average of an integer property of the items in the stream. We also have <code>averagingLong</code>, <code>averagingDouble</code></td><td><code>.collect(averagingInt(Dish::getCalories))</code></td></tr><tr><td><code>summarizingInt</code></td><td><code>IntSummaryStatistics</code></td><td>Returns max, min, count, and average</td><td><code>.collect(summarizingInt(Dish::getCalories))</code></td></tr><tr><td><code>joining</code></td><td>String</td><td>Concatinates the strings, <code>toString</code> of an object</td><td><code>.collect(joining(" "))</code></td></tr><tr><td><code>maxBy</code></td><td><code>Optional&#x3C;T></code></td><td>Maximal element based on the given comparator</td><td><code>.collect(maxBy(Dish::getCalories))</code><br><code>.collect(maxBy((d1, d2) -> d1.getCalories() - d2.getCalories()))</code></td></tr><tr><td><code>minBy</code></td><td><code>Optional&#x3C;T></code></td><td>Minimal element based on comparator</td><td><code>.collect(minBy(Dish::getCalories))</code><br><br><code>.collect(minBy((d1, d2) -> d1.getCalories() - d2.getCalories()))</code></td></tr><tr><td><code>reducing</code></td><td>Type produced by reducing operator</td><td>Reduces the stream into a single value</td><td><code>.collect(reducing(0, Dish::getCalories, Integer::sum))</code></td></tr><tr><td><code>collectingAndThen</code></td><td>Type returning by transforming function</td><td>Wrap a collector and apply a transformation function</td><td><code>.collect(collectingAndThen(toList(), List::size))</code></td></tr><tr><td><code>groupingBy</code></td><td><code>Map&#x3C;K, List&#x3C;T>></code></td><td>Group by a property, multi level grouping is possible</td><td><p><code>.collect(groupingBy(Dish::getType))</code><br><br>Multiple grouping</p><pre><code>collect(groupingBy(Dish::getType, groupingBy(dish -> {
    if (dish.getCalories() &#x3C;= 400) return Dish.CalroicLevel.DIET;
    else if (dish.getCalories() &#x3C;= 700) return Dish.CalroicLevel.NORMAL;
    return Dish.CalroicLevel.FAT;
})))
</code></pre></td></tr><tr><td><code>partitioningBy</code></td><td><code>Map&#x3C;boolean, List&#x3C;T>></code></td><td>returns the <code>true and false</code> group</td><td><code>.collect(partitioningBy(Dish::isVegetarian))</code></td></tr></tbody></table>

#### Collector Interface

{% code overflow="wrap" %}
```java
public interface Collector<T, A, R> {
    Supplier<A> supplier();
    BiConsumer<A, T> accumulator();
    Function<A, R> finisher();
    BinaryOperator<A> combiner();
    Set<Characteristics> characteristics();
}

// T - Generic type of the items in the stream to be collected
// A - type of the accumulator, the object on which the partial result is accumulated during the collection process
// R - type of the object, resulting from the collection

// Example:
public class ToListCollector<T> implements Collector<T, List<T>, List<T>> {}
```
{% endcode %}

`INDENTITY_FINISH` if the `accumulator` output matches the finisher, in that case, we use `Identitiy` and `toList` is one such collector.

In case of `IDENTITY_FINISH` collectors, `collect` provides an overrided method that accepts

`supplier, accumulator, combiner`

`stream.collect(ArrayList::new, List::add, List::addAll)`

#### Default Methods

Java introduced static methods and default methods to the interface

```java
default Predicate<T> and(Predicate<? super T> other) {
    Objects.requireNonNull(other);
    return (t) -> test(t) && other.test(t);
}

static <T> Predicate<T> not(Predicate<? super T> target) {
    Objects.requireNonNull(target);
    return (Predicate<T>)target.negate();
}
```

Default methods help the library developers add breaking changes without impacting the downstream API implementors.

Static methods are utility functions that can be directly added to the interface.

#### Abstract Class vs Interfaces

There are still differences b/w them for instance

* An abstract class can only be inherited by a single class, whereas we can extend multiple interfaces.
* Abstract classes can enforce common state (instance variables), whereas interfaces cannot have instance variables.

#### Resolution Rules

Now that we have default methods, there is always a chance of collision of methods, so there are three resolution rules in place

* Classes always win
* Sub-interfaces win, in other terms (B extends A, B is picked)
* If ambiguity exists, the class should override it

### Optional Interface

Java introduced `Optional` wrapper for a value `Optional<T>`.It is primarily to handle `Null` exceptions.

<table><thead><tr><th width="253">Method</th><th>Description</th></tr></thead><tbody><tr><td>empty</td><td>Returns empty Optional instance</td></tr><tr><td>filter</td><td>If the value is present and matches the given predicate, returns the value, else returns empty one</td></tr><tr><td>flatMap</td><td>If the value is present, returns the Optional resulting from the application of the provided mapping function, else returns empty</td></tr><tr><td>get</td><td>returns the value wrapped by the Optional, otherwise throws <code>NoSuchElementException</code></td></tr><tr><td>ifPresent</td><td>if the value is present, executes the consumer passed</td></tr><tr><td>isPresent</td><td>Returns true, if value is present, else false</td></tr><tr><td>map</td><td>if the value is present, applies the mapping function</td></tr><tr><td>ofNullable</td><td>Returns Optional wrapping of the given value, if the value is null</td></tr><tr><td>orElse</td><td>if the value is not preset, it will return the value supplied in</td></tr><tr><td>orElseGet</td><td>if the value is not present, will get one from the supplier</td></tr><tr><td>orElseThrow</td><td>if there is no value, error is thrown</td></tr></tbody></table>
