# Constructors

## Dart Constructor Peculiarities 

### 1. Named Parameters

To assign values in constructor using parameter names, use the following example:

```dart
class Person {
    String personName;
    int personAge;
    
    Person({String name, int age}) { // Add curly brackets to make named parameters
        personName = name;
        personAge = age;
    }
}

void main() {
    var person = Person(name: 'John Smith', age: 35) // Use parameter name to specify value
}
```



### 2. Default Parameter Values

To assign default values in constructor, use the following example:

```dart
class Person {
    String personName;
    int personAge;
    
    Person(String name, int age = 30) { // Default value of 'age' parameter is 30
        personName = name;
        personAge = age;
    }
}

void main() {
    var person = Person('John Smith');
    print(person.personAge); // Prints '30' if value not specified
}
```



### 3.  Parameter Property Value

To directly assign object property values as parameters, use the following example:

```dart
class Person {
    String personName;
    int personAge;
    
    Person({this.personName, this.personAge = 30}); // 'this' keyword allows object to directly reference its own properties
}

void main() {
    var person = Person(personName: 'John Smith', personAge: 59);
}
```

