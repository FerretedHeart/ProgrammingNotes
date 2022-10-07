# Arrays

Put a regular array into ascending order:

```java
public class Main {
  public static void main(String[] args) {
    int[] numbers = {167, -2, 16, 99, 26, 92, 43, -234, 35, 80};
    for (int i = numbers.length - 1; i > 0; i--) {
      for (int j = 0; j < i; j++) {
        // Compare the elements in pairs.
        // If they are not in the right order, then swap them
        if (numbers[j] > numbers[j + 1]) {
          int tmp = numbers[j];
          numbers[j] = numbers[j + 1];
          numbers[j + 1] = tmp;
        }
      }
    }
  }
}

// or

public class Main {
  public static void main(String[] args) {
    int[] numbers = {167, -2, 16, 99, 26, 92, 43, -234, 35, 80};
    Arrays.sort(numbers);
    System.out.println(Arrays.toString(numbers));
  }
}
```

Note: To convert the array to a string, we used another method of the `Arrays` class: `Arrays.toString()`. Arrays in Java don't override the `toString()` method on their own. So, if you simply write `System.out.println(numbers.toString());`, the Object's class `toString()` will be called. For an array, the output will be something like this: `[l@4554717c]`.

Copying is also easily accomplished with the `Arrays` class:

```java
public class Main {
  public static void main(String[] args) {
    int[] numbers = {167, -2, 16, 99, 26, 92, 43, -234, 35, 80};
    int [] numbersCopy = Arrays.copyOf(numbers, numbers.length);
    System.out.println(Arrays.toString(numbersCopy));
  }
}
```

`ArrayList` can not only find an object by its index, but also vice versa: it can use a reference to find an object's index in the `ArrayList`.

This is what the `indexOf()` method is for:

We pass a reference to the object we want, and `indexOf()` returns its index:

```java
public static void main(String[] args) {
  ArrayList<Cat> cats = new ArrayList<>();
  Cat thomas = new Cat("Thomas");
  Cat behemoth = new Cat("Behemoth");
  Cat lionel = new Cat("Lionel Messi");
  Cat fluffy = new Cat ("Fluffy");
  
  cats.add(thomas);
  cats.add(behemoth);
  cats.add(lionel);
  cats.add(fluffy);
  
  int thomasIndex = cats.indexOf(thomas);
  System.out.println(thomasIndex)
}
// Prints: 0
```

|                                         | Array                           | ArrayList                                         |
| --------------------------------------- | ------------------------------- | ------------------------------------------------- |
| Create a container for elements         | String[] list = new String[10]; | ArrayList<String> list = new ArrayList<String>(); |
| Get the number of elements              | int n = list.length;            | int n = list.size();                              |
| Get an element from an array/collection | String s = list[3];             | String s = list.get(3);                           |
| Write an element into an array          | list[3] = s;                    | list.set(3, s);                                   |

The difference is that inserting using `set()` overwrites the old value.

Inserting using `add()` first shifts by one all elements from [index] to the end of the array, and then adds the specified object in the resulting empty position.

To completely calr the list, we use the `clear()` method:

|                                              | Array                                                        | ArrayList        |
| -------------------------------------------- | ------------------------------------------------------------ | ---------------- |
| Add an element at the end of the array       | This action is not supported                                 | list.add(s);     |
| Add an element in the middle of the array    | This action is not supported                                 | list.add(15, s); |
| Add an element at the beginning of the array | This action is not supported                                 | list.add(0, s);  |
| Delete an element from the array             | We could delete an element with `list[3] = null`. But this would leave a 'hole' in the array. | list.remove(3);  |

In Java, we need a special object called an `iterator` (`Iteractor` class) to delete items while iterating over a collection. The `Iterator` class is responsible for safely iterating over the list of elements. It is quite simple, since it has only 3 methods:

- `hasNext()` - returns true or false, depending on whether there is a next item in the list, or we have already reached the last one
- `next()` - returns the next item in the list
- `remove()` - removes an item from the list

Suppose we want to check if there is a next element in our list, and display it if there is:

```java
Iterator<Cat> catIterator = cats.iterator(); // create an iterator
while(catIterator.hasNext()) { // as long as there are elements in the list
  Cat nextCat = catIterator.next(); // get the next element
  System.out.println(nextCat); // display it
}
```

Remove a cat:

```java
Iterator<Cat> catIterator = cats.iterator(); // create an iterator
while(catIterator.hasNext()) { // as long as there are elements in the list
  Cat nextCat = catIterator.next(); // get the next element
  if(nextCat.name.equals("Lionel Messi")) {
    catIterator.remove(); // delete the cat with the specified name
  }
}

System.out.println(cats);
```

