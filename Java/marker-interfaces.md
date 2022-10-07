# Marker Interfaces

Marker interfaces are interfaces without methods. When a class implements such an interface, we say that it is marked by it.

- **Serializable** - used to mark classes that support serialization, indicating that instances of these classes can be automatically serialized and deserialized
- **Remote** - used to identify objects that support remote execution, i.e. methods that can be invoked from another Java virtual     machine and/or different computer
- **Cloneable** - used to mark classes that support cloning
  - **Shallow copying** - creating a copy of an object, without making duplicates of any of the objects it references

- **Deep copying** - duplicating an object, including objects it references, and the objects that those objects reference, etc.

- - Create a buffer (byte array) in memory
  - Serialize the object and subobjects into the buffer
  - Deserialize the object hierarchy saved in the buffer

```java
BigObject objectOriginal = new BigObject(); //create objectOriginal which will be cloned

ByteArrayOutputStream writeBuffer = new ByteArrayOutputStream(); //create ByteArrayOutputStream which will expand dynamically as new data is added (like an ArrayList)

ObjectOutputStream outputStream = new ObjectOutputStream(writeBuffer); //create an ObjectOutputStream which is used for serialization
outputStream.writeObject(objectOriginal); // serialize objectOriginal into a byte array using outputStream and save it to writeBuffer
outputStream.close();

byte[] buffer = writeBuffer.toByteArray(); //convert writeBuffer into an ordinary byte array
ByteArrayInputStream readBuffer = new ByteArrayInputStream(buffer); //transform buffer into a ByteArrayInputStream in order to read from it like an InputStream
ObjectInputStream inputStream = new ObjectInputStream(readBuffer); // pass readBuffer to the ObjectInputStream constructor to read (deserialize) the object
BigObject objectCopy = (BigObject)inputStream.readObject(); //read our object and convert it to a BigObject
```

