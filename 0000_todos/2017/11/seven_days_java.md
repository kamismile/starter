[TOC]

# passing behavior before java 8

```java
sendEmail(new Runnable() {
  @Override
  public void run() {
    System.out.println("Sending email...");
  }
})
```

# java 8 lambda expressions

```java
sendEmail(() -> System.out.println("sending email..."));
Comparator<String> comparator = (first, second) -> first.length() - second.length();

```

