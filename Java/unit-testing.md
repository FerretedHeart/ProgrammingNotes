# JUnit Testing

## Resources

[JUnit 5](https://junit.org/junit5/)

[Maven Repository](https://mvnrepository.com/artifact/org.junit)

## Getting Started

To create a unit test, right-click on the class and then choose Test, which will prompt you for information for the test class it will create for you.

![image-20231215093459064](C:\Users\Jeanine.Kimball\markdown\ProgrammingNotes\Images\image-20231215093459064.png)



## Annotations

Annotations provide metadata about code but aren't executable statements, and are added above the method.

`@Test` indicates that it is a JUnit 5 test method to execute.

`@BeforeAll` will run once before all of the test methods in the class.

`@BeforeEach` will run once before each test method.

`@AfterEach` will run once after each test method.

`@AfterAll` will run once after all test methods in the class have completed.

`@DisplayName` will allow for a different name to display instead of the class or method name.

`@Nested` allows to nest test classes.

`@Disabled` ignores the test method.

`@Tag("dateTime")` so that you can create a run/debug configuration based on that tag to run those specific tests (grouping). It can be applied to either the test class and/or the test methods.

## Assertions

All assertions will allow you to include a message at the end of the assertion parameters for clarity of failures, as either `message:` or as a lambda `() ->`.

`assertNotNull()` operation where if the info is null, the test fails.

`assertEquals(expected value, test value)` tests if the values are the same.

`assertSame(expected value, test value)` tests if the values are not only the same but that they are the same object.

`assertThrows(RuntimeException.class, () -> test value)` explicitly verify that we get an exception when certain code is called. What we expect back is a runtime exception.

```java
@Test
void throwExceptionIfIncorrectPatternProvided() {
    Throwable error = assertThrows(RuntimeException.class, () -> DateTimeConverter.convertStringToDateTime("9/2/2018 100pm", 		LocalDate.of(2018, 9, 1)));
    assertEquals("Unable to create date time from: [9/2/2018 100pm], " + "please enter with format [M/d/yyy h:mm a], Text '9/2/2018 100 PM'" + "could not be parsed at index 12", error.getMessage());
}
```

`assertAll(() => all of the assertion tests)`  allows for all test method assertions to run and provide results on all failures rather than just the first one found.