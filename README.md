# Date and Time API

## Learning Goals

- Learn about the Date and Time API that Java introduced in Java 8.
- Look at the `LocalDate`, `LocalTime`, and `LocalDateTime` classes.

## Introduction

When Java 8 rolled out, a new Date and Time API was also introduced. All the
classes in the new Date and Time API can be found in the `java.time` package.
In this lesson, we will mostly go over the `LocalDate` class that is part of
the `java.time` package, and we'll compare it to the legacy `Date` and
`Calendar` class that we reviewed in the previous lesson. We'll also mention
the `LocalTime` class and the `LocalDateTime` class briefly in this lesson as
well.

## Java's `LocalDate` Class

The `LocalDate` class is an immutable class that only holds the date values of
a component without a time or a timezone associated with it. A good use case of
a `LocalDate` could be a birthday.

### Constructing a `LocalDate`

Let's say we have a `Person` class and the `Person` class, for this example,
only has a name and a birthday:

```java
import java.time.LocalDate;

public class Person {
    
    private String name;
    private LocalDate birthday;
    
    public static void main(String[] args) {
        Person ben = new Person();
    }
}
```

Let's say we want to specify the current date. To do so, we can use the static
`now()` method which will return a `LocalDate` instance with the current date:

```java
import java.time.LocalDate;

public class Person {

    private String name;
    private LocalDate birthday;

    public Person() {
        this.name = "";
        this.birthday = LocalDate.now();
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public LocalDate getBirthday() {
        return birthday;
    }

    public static void main(String[] args) {
        Person ben = new Person();
        System.out.println(ben.getBirthday());    // <-- this will print out the current date
    }
}
```

For example, if the current date is August 18, 2022, then the following would be
the output of the above code:

```plaintext
2022-08-18
```

But assuming our `Person` instance was not born today, let's specify the month,
day, and year they were born!

```java
import java.time.LocalDate;

public class Person {

    private String name;
    private LocalDate birthday;

    public Person() {
        this.name = "";
        this.birthday = LocalDate.now();
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public LocalDate getBirthday() {
        return birthday;
    }

    public void setBirthday(int year, int month, int dayOfMonth) {
        this.birthday = LocalDate.of(year, month, dayOfMonth);
    }

    public static void main(String[] args) {
        Person ben = new Person();
        ben.setName("Ben Wyatt");
        ben.setBirthday(1974, 11, 14);
        System.out.println(ben.getBirthday());
    }
}
```

In the above example, notice how we can specify a certain `LocalDate` using the
static method `of()` that, in this case, will take in an `int` for the year, an
`int` for the month, and an `int` for the day of the month. Also note that,
unlike the `Calendar` class we saw, the months actually begin with January == 1,
February == 2, March == 3, ..., and December == 12 - which is how we normally
number the months of the year! This clears up some of that confusion we might
have had before!

We could have also created the `setBirthday()` method to take in a `Month`
instead of an integer representation of a month. `Month` is an enum that is also
part of the `java.time` package that we can use to specify any of the twelve
months of the year.

```java
public void setBirthday(int year, Month month, int dayOfMonth) {
    this.birthday = LocalDate.of(year, month, dayOfMonth);
}
```

If we were to use the `setBirthday()` method taking in a `Month` enum instead of
an integer representation of the month, then we could call the method like this:

```java
ben.setBirthday(1974, Month.NOVEMBER, 14);
System.out.println(ben.getBirthday());
```

Both of these `of()` methods would return a `LocalDate` object like this:

```plaintext
1974-11-14
```

### Get Date Components

Like the `Calendar` class, we can get the date components of a `LocalDate`
object as well using the getter methods below:

| Method          | Description                                                            |
|-----------------|------------------------------------------------------------------------|
| getDayOfMonth() | Gets the day of the month                                              |
| getMonth()      | Gets the enum `Month` (e.g. Month.APRIL)                               |
| getMonthValue() | Gets the integer representation of a month (e.g. 4 to represent April) |
| getYear()       | Gets the year                                                          |

Using the `Person` class above, let's test out some of these methods!

```java
public static void main(String[] args) {
    Person ben = new Person();
    ben.setName("Ben Wyatt");
    ben.setBirthday(1974, Month.NOVEMBER, 14);

    System.out.println(ben.getBirthday().getMonth());
    System.out.println(ben.getBirthday().getMonthValue());
    System.out.println(ben.getBirthday().getDayOfMonth());
    System.out.println(ben.getBirthday().getYear());
}
```

The output of the above would be:

```plaintext
NOVEMBER
11
14
1974
```

### Plus and Minus

Some other methods that might be useful to us using the `LocalDate` class could
be the plus and minus methods. These methods will either add or subtract from
the `LocalDate` instance (and return a `LocalDate` object with the specified
changes).

Let's first look at some methods that add to the `LocalDate` object:

```java
public static void main(String[] args) {
    Person ben = new Person();
    ben.setName("Ben Wyatt");
    ben.setBirthday(1974, Month.NOVEMBER, 14);
    System.out.println(ben.getBirthday());

    /* This will add 30 days to the LocalDate object; we can specify any number of days to add.
       Note that Ben's birthday will not change - the adjusted birthday will have the adjustment
     */
    LocalDate adjustedBirthday1 = ben.getBirthday().plusDays(30);
    System.out.println(adjustedBirthday1);

    // This will add 2 months to the LocalDate object; we can specify an number of months to add.
    adjustedBirthday1 = adjustedBirthday1.plusMonths(2);
    System.out.println(adjustedBirthday1);

    // This will add 10 weeks to the LocalDate object; we can specify an number of weeks to add.
    adjustedBirthday1 = adjustedBirthday1.plusWeeks(10);
    System.out.println(adjustedBirthday1);

    // This will add 5 years to the LocalDate object; we can specify an number of years to add.
    adjustedBirthday1 = adjustedBirthday1.plusYears(5);
    System.out.println(adjustedBirthday1);
}
```

The result of the code above would look like this:

```plaintext
1974-11-14
1974-12-14
1975-02-14
1975-04-25
1980-04-25
```

Now we'll look at the methods where we subtract from the `LocalDate` object:

```java
    public static void main(String[] args) {

        Person ben = new Person();
        ben.setName("Ben Wyatt");
        ben.setBirthday(1974, Month.NOVEMBER, 14);
        System.out.println(ben.getBirthday());

        LocalDate adjustedBirthday1 = ben.getBirthday().plusDays(30);
        adjustedBirthday1 = adjustedBirthday1.plusMonths(2);
        adjustedBirthday1 = adjustedBirthday1.plusWeeks(10);
        adjustedBirthday1 = adjustedBirthday1.plusYears(5);
        System.out.println(adjustedBirthday1);
        
        // Let's get the adjustedBirthday back to the original birthday

        /* This will subtract 5 years from the LocalDate object; we can specify any number of years to subtract.
           Note that, like the plus methods above, the adjustedBirthday1 object will not change.
           Insteaad, the LocalDate, adjustedBirthday2, will hold the changes.     
         */
        LocalDate adjustedBirthday2 = adjustedBirthday1.minusYears(5);
        System.out.println(adjustedBirthday2);

        // This will subtract 10 weeks from the LocalDate object; we can specify any number of weeks to subtract.
        adjustedBirthday2 = adjustedBirthday2.minusWeeks(10);
        System.out.println(adjustedBirthday2);

        // This will subtract 2 months from the LocalDate object; we can specify any number of months to subtract.
        adjustedBirthday2 = adjustedBirthday2.minusMonths(2);
        System.out.println(adjustedBirthday2);

        // This will subtract 30 days from the LocalDate object; we can specify any number of days to subtract.
        adjustedBirthday2 = adjustedBirthday2.minusDays(30);
        System.out.println(adjustedBirthday2);
    }
```

The result of the code above will now look like this:

```plaintext
1974-11-14
1980-04-25
1975-04-25
1975-02-14
1974-12-14
1974-11-14
```

As a review of the methods we demonstrated above, consider the following table
below:

| Method                             | Description                                                                       |
|------------------------------------|-----------------------------------------------------------------------------------|
| minusDays(long daysToSubtract)     | Returns a copy of this `LocalDate` with the specified number of days subtracted   |
| minusMonths(long monthsToSubtract) | Returns a copy of this `LocalDate` with the specified number of months subtracted |
| minusWeeks(long weeksToSubtract)   | Returns a copy of this `LocalDate` with the specified number of weeks subtracted  |
| minusYears(long yearsToSubtract)   | Returns a copy of this `LocalDate` with the specified number of years subtracted  |
| plusDays(long daysToAdd)           | Returns a copy of this `LocalDate` with the specified number of days added        |
| plusMonths(long monthsToAdd)       | Returns a copy of this `LocalDate` with the specified number of months added      |
| plusWeeks(long weeksToAdd)         | Returns a copy of this `LocalDate` with the specified number of weeks added       |
| plusYears(long yearsToAdd)         | Returns a copy of this `LocalDate` with the specified number of years added       |

## `LocalTime` and `LocalDateTime` Classes

For more on the `LocalDate` class, please see the Java documentation here:
[Java 17 LocalDate Class](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/time/LocalDate.html).

As we saw above, the `LocalDate` class only concerns itself with dates rather
than times. This is great if we aren't interested in storing times values!

But what if we only want time values, and we don't really care about the date
itself? This is when we can use the `LocalTime` class. The `LocalTime` class is
an immutable class that only holds the time values of a component without a date
or a timezone associated with it. We can represent a `LocalTime` with nanosecond
precision.

```java
import java.time.LocalTime;

public class LocalTimeExample {
    
    public static void main(String[] args) {
        
        // Will get a LocalTime of the current time to the nanosecond
        LocalTime time = LocalTime.now();
        System.out.println(time);    // <-- May print something like this 12:50:07.319447
    }
}
```

The above example will obtain the current time and store it in the `LocalTime`
instance, `time`. We could also specify the time using a static `of()` method,
similar to how we could define a `LocalDate` object of a specific date.

```java
import java.time.LocalTime;

public class LocalTimeExample {
    
    public static void main(String[] args) {
        
        // Obtain an instance of LocalTime from an hour and minute
        LocalTime time = LocalTime.of(6, 17);
        System.out.println(time);    // <-- Will print 06:17
    }
}
```

So what if we care about both the time and the date values? Then we can use a
`LocalDateTime` object instead which will store both the date and time without a
timezone association. `LocalDateTime` is also an immutable date-time object and
can represent the time with nanosecond precision.

```java
import java.time.LocalDateTime;

public class LocalDateTimeExample {
    
    public static void main(String[] args) {
        
        // Will get a LocalDateTime of the current date-time to the nanosecond
        LocalDateTime dateTime = LocalDateTime.now();
        System.out.println(dateTime);    // <-- May print something like this: 2022-08-18T12:50:07.319447
        
        // Obtain an instance of LocalDateTime by taking in a year, month, day, hour, minute, and second as arguments
        dateTime = LocalDateTime.of(1955, 11, 5, 6, 15, 0);
        System.out.println(dateTime);    // <-- Will print 1955-11-05T06:15
    }
}
```

As we can see by examples above, the `LocalTime` and `LocalDateTime` classes
both have very similar methods to the `LocalDate` class and can assume that their
functionality is also similar. For more on the classes we reviewed in this
lesson, please see the official Java documentation below in the resources
section.

## Resources

- [Java 17 Month Enum](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/time/Month.html)
- [Java 17 LocalDate Class](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/time/LocalDate.html)
- [Java 17 LocalTime Class](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/time/LocalTime.html)
- [Java 17 LocalDateTime Class](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/time/LocalDateTime.html)
