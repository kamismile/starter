[TOC]

[7 days with java 8](https://shekhargulati.com/7-days-with-java-8/)

[java 8 the missing tutorial](https://github.com/shekhargulati/java8-the-missing-tutorial)

[ninety-nine problems in java 8](https://github.com/shekhargulati/99-problems/tree/master/java8)

[getting started with bootique](http://bootique.io/docs/0/getting-started/index.html)



# lambda

## passing behavior before java 8

```java
sendEmail(new Runnable() {
  @Override
  public void run() {
    System.out.println("Sending email...");
  }
})
```

## java 8 lambda expressions

```java
sendEmail(() -> System.out.println("sending email..."));
Comparator<String> comparator = (first, second) -> first.length() - second.length();

```

FunctionalInterface会在编译时校验

## using invokedynamic

## do i need to write my own functional interface

## java.util.function.Predicate

```java
// takes a value and returns boolean
Predicate<String> namesStartingWithS = name -> name.startsWith("s");
```

## java.util.function.Consumer

```java
// takes a a value but produces not value
Consumer<String> messageConsumer = message -> System.out.println(message);
```

## java.util.function.Function

```java
// takes one value and produces a result
Function<String, String> toUpperCase = name -> name.toUpperCase();

```

## Method references

```java
Function<String, Integer> strToLength = str -> str.length();
Function<String, Integer> strToLength = String::length;
```

## static method references

```java
Function<String, String> maxFn = Collections::max;
```

## instance method references

```java
BiFunction<String, String, String> concatFn = String::concat;
concatFn.apply("shekhar", "gulati");

```



# streams

```java
public static void main(String[] args) {
  List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
  List<Integer> evenNumbers = new ArrayList<>();
  
  for (Integer number : numbers) {
    if (number % 2 == 0) {
		evenNumbers.add(number);      
    }
  }
  System.out.println(evenNumbers);
}

public static void main(String[] args) {
  List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
  List<Integer> evenNumbers = numbers.stream().filter(number -> number % 2 == 0)
    .collect(Collectors.toList());
  System.out.println(evenNumbers);
}
```

## external iteration vs internal iteration

## lazy evaluation







