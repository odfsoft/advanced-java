# Spring boot Testing

## What You Will Build
Unit and integration testing for the greeting application you have built before.

## What You Need
- JDK 11 or later
- Maven 3.2+

## Unit testing of the GreetingService
1. create a new java class inside the folder `src/test/java/com/redi/demo/service` with the name `GreetingServiceTest`
2. Define a variable of type `GreetingServiceTest` which we will test.
3. add a setup method to instantiate the `GreetingService` also annotate it with `@BeforeEach`
4. create a new test method to greet `testing`
5. validate that the result of should be equal to `Hello, testing!`
6. add a tearDown method and annotate it with `@AfterEach` to delete the instance of `GreetingService`

```java
class GreetingServiceTest {

    GreetingService greetingService;

    @BeforeEach
    public void setup() {
        greetingService = new GreetingService();
    }

    @Test
    public void testGreetShouldGreatWithName() {
        //Arrange
        String greetName = "testing";

        //Act
        Greeting result = greetingService.greet(greetName);

        //Assert
        assertThat(result.getContent()).isEqualTo("Hello, testing!");
    }
    
    @AfterEach
    public void tearDown() {
        greetingService = null;
    }
}
```
7. right click over the method name and select Run.
8. you can also run the whole test suit by typing in the console
```shell
mvn test
```
or
```shell
mvn clean build
```
## Assertions
assertion allows us to perform different types of validations over the result of our test method.

assertions are of different types for example:
- Equality
- Boolean
- Nullability
- Exceptions
- Lists and others.
```java
class GreetingServiceTest {
    //...
    @Test
    public void testAssertions() {
        int value = 10;
    
        //equals
        assertEquals(10, value);
        assertThat(value).isEqualTo(10);
    
        //boolean
        assertTrue(value == 10);
        assertThat(value == 10).isTrue();
        assertFalse(value == 9);
        assertThat(value == 9).isFalse();
    
        //nullability
        assertNull(value);
        assertThat(value).isNull();
        assertNotNull(value);
        assertThat(value).isNotNull();
    }
}
```
you can find all the documentation about assertions in this [link](https://junit.org/junit5/docs/current/user-guide/)

## First Activity
Create the following test cases for the statistics method:
1. when no greet has been done the statistics array should be empty.
2. when greeting a name the statistics response array size is equal to 1, the value for that name should be equal to 1.
3. when greeting a name twice the statistics response array size should be equal to 1, the value for that name should be equal to 2
4. if we greet 10 times the same name the statistics the response array size should be equal to 1, the value for that name should be equal to 1
5. test that greeting 5 different names 4 times each, returns a statistics array with 5 items and each with a value of 4.

## testing exceptions
we can also assert if our code gives an exception by using the `assertThrows` method. this method receive two parameters a Exception class and a provider which can be a lambda.

in this example we are asserting that our greet method will throw an exception if there has been more than 10 different greets to our greet Services.
```java
class GreetingServiceTest {
    //...
    @Test
    public void shouldGiveAnErrorAfter10DifferentGreets() {
        //Arrange
        String greetName = "testing1234";

        //Act & Arrange
        assertThrows(RuntimeException.class, () -> greetingService.greet(greetName));
    }
}
```
1. Modify the greeting endpoint to cover this new feature.
2. make sure all the other test cases are still working

## Parametrized tests
when you want to test really similar behaviours, but we don't want to have code duplication, we can use parametrized tests

value source is for primitives and simple classes like String, but if you need a custom class like Greetings then you can use MethodSource.

other types are:
- EnumSource
- CSVSource
  -ArgumentsSource

1. add the following value source example to your test class
```java
class GreetingServiceTest {
    //...
    @ValueSource(strings = {"Berlin", "REDI Rocks", "My Name"})
    @ParameterizedTest
    public void shouldGreetWithValueSource(String greetName) {
        //Arrange

        //Act &
        Greeting result = greetingService.greet(greetName);

        //Arrange
        assertThat(result.getContent()).isEqualTo("Hello, " + greetName + "!");
    }
}
```
2. add the following method source example to your test class
```java
class GreetingServiceTest {
    //...
    @MethodSource("data")
    @ParameterizedTest
    public void shouldGreetWithMethodSource(String greetName) {
        //Arrange

        //Act &
        Greeting result = greetingService.greet(greetName);

        //Arrange
        assertThat(result.getContent()).isEqualTo("Hello, " + greetName + "!");
    }

    public static String[] data() {
        return new String[] { "Berlin", "REDI Rocks", "My Name" };
    }
}
```


