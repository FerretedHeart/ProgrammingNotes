# Serialization

- [Singleton and Serialization](https://javarevealed.wordpress.com/tag/singleton-and-serialization/)
- [Serialization and deserialization](https://codegym.cc/groups/posts/112-serialization-and-deserialization-in-java)
- [Externalizable interface](https://codegym.cc/groups/posts/113-introducing-the-externalizable-interface)
- [SerialVersionUID](https://java2blog.com/serialversionuid-in-java-serialization/)
- [Object Serialization with inheritance](https://www.geeksforgeeks.org/object-serialization-inheritance-java/)

 Take an object and save it to a stream and read from a stream:

```java
public static void main(String[] args) throws Exception {
  Cat cat = new Cat();
  //Save a cat to file
  FileOutputStream fileOutput = new FileOutputStream("cat.dat");
  ObjectOutputStream outputStream = new ObjectOutputStream(fileOutput);
  outputStream.writeObject(cat);
  fileOutput.close();
  outputStream.close();
  //Load a cat from file*
  FileInputStream fiStream = new FileInputStream("cat.dat");
  ObjectInputStream objectStream = new ObjectInputStream(fiStream);
  Object object = objectStream.readObject();
  fiStream.close();
  objectStream.close();
  Cat newCat = (Cat)object;
}
```

Java's creators came up with the special Serializable interface marker. It's called a marker, because it doesn't contain any data and methods. It's only used to "tag" or "mark" classes. If we believe that our class stores all its data internally, then we can mark it with implements Serializable.

Moreover, an object's type is saved when the object is serialized. Now you can save a reference to a Cat object in an Object variable. Everything will serialize and deserialize just fine. Deserialization is the process of reversing serialization: reading and reconstructing an object from a stream/file.

If the class stores data that doesn't play a significant role in its state and yet prevents the class from being considered a serializable class, then the transient keyword is used. If we write this keyword before a member variable, then it will be ignored during serialization. Its state won't be saved or reconstructed. As if it didn't exist.

```java
class Cat implements Serializable {
  public String name; 
  public int age;
  public int weight;
  
  transient public InputStream in = System.in;
}
```

Sometimes you need to control the serialization process. Here are some of reasons why:

1. An object is not ready for serialization: its current internal state is in the process of changing.
2. An object contains non-serializable objects, but can convert them into a form that can be easily serialized, e.g. save them as a byte array or something else.
3. An object wants to deserialize all its data as one unit and/or to encrypt it before serialization.

Simply replace the Serializable interface with the Externalizable interface, and your class can manage the serialization process manually.

```java
class Cat implements Externalizable {
  public String name;
  public int age;
  public int weight;
  
  public void writeExternal(ObjectOutput out) {
    out.writeObject(name);
    out.writeInt(age);
    out.writeInt(weight);
  }
  
  public void** readExternal(ObjectInput in) {
    name = (String) in.readObject();
    age = in.readInt();
    weight = in.readInt();
  }
}
```

 