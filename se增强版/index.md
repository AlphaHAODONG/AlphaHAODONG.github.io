# SE增强版




### 类加载器

类加载器（Class Loader）是 Java 虚拟机（JVM）的一个重要组成部分，它负责将类的字节码加载到 JVM 中，并将其转换为运行时的实例。类加载器实际上是一个 Java 类，它通过查找类的字节码文件并创建类的定义来完成加载过程。

Java 类加载器的主要功能包括以下几个方面：

\1. 加载（Loading）：类加载器负责在运行时查找并加载类的字节码文件。它根据类的名称或其他标识符，从文件系统、网络或其他来源获取类的字节码，并创建一个表示该类的 Class 对象。

\2. 验证（Verification）：类加载器在加载类的过程中进行验证，确保类的字节码文件符合 Java 虚拟机规范，不会造成安全问题或导致系统崩溃。

\3. 准备（Preparation）：准备阶段是类加载过程的一部分，其中类加载器为类的静态变量分配内存并设置默认初始值。

\4. 解析（Resolution）：解析阶段是类加载过程的一部分，其中类加载器将符号引用解析为直接引用。符号引用是一种符号名称，它在编译时用于引用类、方法、字段等，而直接引用是对实际内存地址的引用。

\5. 初始化（Initialization）：初始化阶段是类加载过程的最后一步，在该阶段，类的静态变量被分配所定义的初始值，并执行类的静态初始化块。

\6. 使用（Usage）：类加载器加载的类可以被 JVM 使用，包括创建类的实例、调用类的方法等。

Java 类加载器采用了双亲委派模型（Parent Delegation Model）来组织类加载器之间的关系。根据这个模型，当一个类加载器收到类加载请求时，它首先将该请求委派给父加载器进行处理。只有当父加载器无法找到所请求的类时，子加载器才会尝试加载该类。这种层级关系保证了类的一致性和安全性。

常见的类加载器包括以下几种：

\1. 启动类加载器（Bootstrap Class Loader）：负责加载 Java 核心库（rt.jar）等核心类，是最顶层的类加载器，通常由 JVM 实现，并不是一个普通的 Java 类。

\2. 扩展类加载器（Extension Class Loader）：负责加载 Java 扩展库（jre/lib/ext目录下的JAR文件）中的类。

\3. 应用程序类加载器（Application Class Loader）：也称为系统类加载器，负责加载应用程序类路径（Classpath）下的类，开发者自定义的类通常由该类加载器加载。

此外，还可以自定义类加载器来加载非标准的类文件，实现特定的加载需求，如加载网络资源、动态生成类等。

类加载器在 Java 应用程序的运行过程中起着重要的作用，它通过动态加载类的方式提供了灵活性和扩展性，使得应用程序能够根据需要加载和使用不同的类和模块。了解类加载器的工作原理和机制有助于编写更高效、安全和可扩展的 Java 代码。





### 双亲委派模型

双亲委派模型（Parent Delegation Model）是 Java 类加载器的一种工作机制，它是为了解决类加载的安全性和一致性问题而引入的。

在双亲委派模型中，类加载器之间形成了一个层级关系，每个类加载器都有一个父加载器（除了顶层的启动类加载器）。当一个类加载器需要加载某个类时，它首先将加载请求委派给其父加载器进行处理，只有当父加载器无法找到该类时，才由子加载器尝试加载。这个过程被称为委派（Delegation）。

具体的委派过程如下：

\1. 当一个类加载器收到加载请求时，它首先检查自己是否已经加载了该类。如果已经加载，则直接返回已加载的 Class 对象。

\2. 如果该类尚未加载，则委派给父加载器进行加载。父加载器也按照相同的方式进行检查，首先检查自己是否已经加载了该类，如果已经加载，则直接返回 Class 对象。

\3. 如果父加载器无法加载该类，则再次将加载请求委派给父加载器的父加载器，依次往上委派，直到达到顶层的启动类加载器。

\4. 如果顶层的启动类加载器仍无法加载该类，则子加载器尝试自己加载该类。

通过这种委派机制，类加载器可以确保类的一致性和安全性。它可以防止不同的加载器重复加载同一个类，保证类的唯一性。同时，由于父加载器优先加载，可以确保核心类库由启动类加载器加载，从而保证了核心类库的安全性。

双亲委派模型的优势在于它能够有效地避免类的重复加载和冲突，提高了类加载的效率和安全性。它也为自定义类加载器提供了灵活的扩展机制，可以根据具体需求实现特定的类加载行为，例如加载非标准的类文件或动态生成类。

需要注意的是，虽然双亲委派模型是 Java 类加载器的默认行为，但在某些情况下可以通过自定义类加载器来打破这种机制，实现特定的加载需求。

### 反射

反射（Reflection）是一种在运行时检查和操作类、对象、方法和属性的能力。Java的反射机制提供了一组API，允许开发人员在运行时获取和使用类的信息，以及动态地调用类的方法、访问和修改对象的属性。

通过反射，可以在运行时获取类的各种信息，包括类的名称、修饰符、父类、实现的接口等。它使得程序可以在编译时不知道具体类的情况下操作类和对象。反射提供了以下常见的类和接口：

**\1. `Class`类：`java.lang.Class`类是反射的核心类之一。每个类在内存中都有一个对应的`Class`对象，它提供了获取类的信息和执行反射操作的方法。通过`Class`类可以获取类的构造函数、字段、方法等信息，以及创建类的实例对象。**

1. `getName()`：获取类的全限定名（包括包名和类名）。

  示例：

```java
  Class<?> clazz = MyClass.class;
  String className = clazz.getName(); // 获取类名
```

  

2. `getSimpleName()`：获取类的简单名称（只包括类名）。

  示例：

```java
  Class<?> clazz = MyClass.class;
  String simpleName = clazz.getSimpleName(); // 获取简单类名
```

3. `getPackage()`：获取类所在的包。

  示例：

```java
  Class<?> clazz = MyClass.class;
  Package pkg = clazz.getPackage(); // 获取包信息
```

4. `getSuperclass()`：获取类的父类。

  示例：

  

```java
Class<?> clazz = MyClass.class;
  Class<?> superclass = clazz.getSuperclass(); // 获取父类信息
```

5. `getInterfaces()`：获取类实现的接口。

  示例：

 

```java
 Class<?> clazz = MyClass.class;
  Class<?>[] interfaces = clazz.getInterfaces(); // 获取实现的接口信息
```

 

6. `getModifiers()`：获取类的修饰符。

  示例：

```java
  Class<?> clazz = MyClass.class;
  int modifiers = clazz.getModifiers(); // 获取修饰符
```

  

7. `getFields()`：获取类的公共字段（包括父类的公共字段）。

  示例：

```java
  Class<?> clazz = MyClass.class;
  Field[] fields = clazz.getFields(); // 获取公共字段
```

8. `getDeclaredFields()`：获取类声明的所有字段（包括私有字段）。

  示例：

```java
  Class<?> clazz = MyClass.class;
  Field[] declaredFields = clazz.getDeclaredFields(); // 获取所有字段
```

9. `getMethods()`：获取类的公共方法（包括父类的公共方法）。

  示例：

```java
  Class<?> clazz = MyClass.class;
  Method[] methods = clazz.getMethods(); // 获取公共方法
```

10. `getDeclaredMethods()`：获取类声明的所有方法（包括私有方法）。

  示例：

```java
  Class<?> clazz = MyClass.class;
    Method[] declaredMethods = clazz.getDeclaredMethods(); // 获取所有方法
```

 

11. `getConstructors()`：获取类的公共构造函数。

  示例：

```java
  Class<?> clazz = MyClass.class;
  Constructor<?>[] constructors = clazz.getConstructors(); // 获取公共构造函数
```

12. `getDeclaredConstructors()`：获取类声明的所有构造函数。

  示例：

```java
  Class<?> clazz = MyClass.class;
  Constructor<?>[] declaredConstructors = clazz.getDeclaredConstructors(); // 获取所有构造函数
```

  

这些方法可以通过`Class`对象来获取类的相关信息，如类名、包名、父类、接口、字段、方法和构造函数等。通过这些信息，可以实现动态地操作类和对象，例如创建对象、调用方法和修改字段值等。需要根据具体需求选择合适的方法来获取所需的类信息。







**2. `Constructor`类：`java.lang.reflect.Constructor`类代表类的构造函数。通过`Constructor`类可以获取构造函数的信息，包括名称、参数类型、修饰符等。可以使用`Constructor`类创建类的实例对象。**

`Constructor`类提供了以下功能和方法：

1. 获取构造方法信息：`Constructor`类可以提供关于构造方法的详细信息，例如构造方法的名称、参数列表、修饰符等。
   \- `getName()`：获取构造方法的名称。
   \- `getParameterTypes()`：获取构造方法的参数类型，返回一个`Class`数组。
   \- `getModifiers()`：获取构造方法的修饰符，返回一个代表修饰符的整数值。
   \- `isVarArgs()`：判断构造方法是否使用了可变参数。
   \- `getExceptionTypes()`：获取构造方法声明抛出的异常类型，返回一个`Class`数组。

2. 创建实例：`Constructor`类可以用于创建类的实例。
   \- `newInstance(Object... initargs)`：通过构造方法创建类的实例，并传递初始化参数。

下面是一个示例，展示了如何使用`Constructor`类来获取和使用构造方法：

```java
import java.lang.reflect.Constructor;

public class ReflectionExample {
  public static void main(String[] args) {
    try {
      // 获取类的构造方法
      Class<?> clazz = MyClass.class;
      Constructor<?> constructor = clazz.getConstructor(String.class, int.class);

​      // 获取构造方法信息
​      String constructorName = constructor.getName();
​      Class<?>[] parameterTypes = constructor.getParameterTypes();
​      int modifiers = constructor.getModifiers();

​      // 输出构造方法信息
​      System.out.println("Constructor Name: " + constructorName);
​      System.out.println("Parameter Types: ");
​      for (Class<?> parameterType : parameterTypes) {
​        System.out.println(parameterType.getName());
​      }
​      System.out.println("Modifiers: " + modifiers);

​      // 创建实例
​      Object instance = constructor.newInstance("example", 10);
​      System.out.println("Instance created: " + instance);
​    } catch (Exception e) {
​      e.printStackTrace();
​    }
  }
}

class MyClass {
  public MyClass(String name, int value) {
    // 构造方法的实现
  }
}
```

在上面的示例中，我们使用反射获取了`MyClass`类的构造方法，并输出了构造方法的名称、参数类型和修饰符信息。然后，通过构造方法创建了一个`MyClass`类的实例。







**3. `Field`类：`java.lang.reflect.Field`类代表类的字段。通过`Field`类可以获取字段的信息，包括名称、类型、修饰符等。可以使用`Field`类访问和修改对象的字段值。**

`Field`类提供了以下功能和方法：

\1. 获取字段信息：`Field`类可以提供关于字段的详细信息，例如字段的名称、类型、修饰符等。
  \- `getName()`：获取字段的名称。
  \- `getType()`：获取字段的类型，返回一个`Class`对象。
  \- `getModifiers()`：获取字段的修饰符，返回一个代表修饰符的整数值。

\2. 访问和修改字段值：`Field`类提供了访问和修改字段值的方法。
  \- `get(Object obj)`：获取指定对象上该字段的值。
  \- `set(Object obj, Object value)`：将指定对象上该字段的值设置为给定的值。

下面是一个示例，展示了如何使用`Field`类来获取和修改字段值：

```java
import java.lang.reflect.Field;

public class ReflectionExample {
  public static void main(String[] args) {
    try {
      // 获取字段信息
      Class<?> clazz = MyClass.class;
      Field field = clazz.getDeclaredField("myField");

​      // 获取字段信息
​      String fieldName = field.getName();
​      Class<?> fieldType = field.getType();
​      int modifiers = field.getModifiers();

​      // 输出字段信息
​      System.out.println("Field Name: " + fieldName);
​      System.out.println("Field Type: " + fieldType.getName());
​      System.out.println("Modifiers: " + modifiers);

​      // 创建实例并访问字段值
​      Object instance = clazz.getDeclaredConstructor().newInstance();
​      field.setAccessible(true); // 设置可访问私有字段
​      Object fieldValue = field.get(instance);
​      System.out.println("Field Value: " + fieldValue);

​      // 修改字段值
​      field.set(instance, "New Value");
​      System.out.println("Modified Field Value: " + field.get(instance));
​    } catch (Exception e) {
​      e.printStackTrace();
​    }
  }
}

class MyClass {
  private String myField;

  // Getter and setter for myField (not shown for brevity)
}
```

在上面的示例中，我们使用反射获取了`MyClass`类的字段，并输出了字段的名称、类型和修饰符信息。然后，我们创建了一个`MyClass`类的实例，并使用`Field`类获取和修改了字段的值。

请注意，在访问私有字段时，我们需要调用`setAccessible(true)`方法将字段设置为可访问状态，以便获取或修改其值。



**4. `Method`类：`java.lang.reflect.Method`类代表类的方法。通过`Method`类可以获取方法的信息，包括名称、参数类型、返回类型、修饰符等。可以使用`Method`类调用类的方法。**

`Method`类提供了以下功能和方法：

1. 获取方法信息：`Method`类可以提供关于方法的详细信息，例如方法的名称、返回类型、参数列表、修饰符等。
   \- `getName()`：获取方法的名称。
   \- `getReturnType()`：获取方法的返回类型，返回一个`Class`对象。
   \- `getParameterTypes()`：获取方法的参数类型，返回一个`Class`数组。
   \- `getModifiers()`：获取方法的修饰符，返回一个代表修饰符的整数值。

2. 调用方法：`Method`类提供了调用方法的方法。
   \- `invoke(Object obj, Object... args)`：在指定的对象上调用方法，可以传递方法的参数。

下面是一个示例，展示了如何使用`Method`类来获取方法信息和调用方法：

```java
import java.lang.reflect.Method;

public class ReflectionExample {
  public static void main(String[] args) {
    try {
      // 获取方法信息
      Class<?> clazz = MyClass.class;
      Method method = clazz.getMethod("myMethod", int.class, String.class);

​      // 获取方法信息
​      String methodName = method.getName();
​      Class<?> returnType = method.getReturnType();
​      Class<?>[] parameterTypes = method.getParameterTypes();
​      int modifiers = method.getModifiers();

​      // 输出方法信息
​      System.out.println("Method Name: " + methodName);
​      System.out.println("Return Type: " + returnType.getName());
​      System.out.println("Parameter Types: ");
​      for (Class<?> parameterType : parameterTypes) {
​        System.out.println(parameterType.getName());
​      }
​      System.out.println("Modifiers: " + modifiers);

​      // 创建实例并调用方法
​      Object instance = clazz.getDeclaredConstructor().newInstance();
​      Object result = method.invoke(instance, 10, "example");
​      System.out.println("Method Result: " + result);
​    } catch (Exception e) {
​      e.printStackTrace();
​    }
  }
}

class MyClass {
  public String myMethod(int value, String text) {
    // 方法的实现
    return text + value;
  }
}
```

在上面的示例中，我们使用反射获取了`MyClass`类的方法，并输出了方法的名称、返回类型、参数类型和修饰符信息。然后，我们创建了一个`MyClass`类的实例，并使用`Method`类调用了该方法，并获取了方法的返回值。

需要注意的是，在调用方法时，我们需要提供方法的参数，并将其作为参数传递给`invoke`方法。

请注意，如果要调用私有方法或者方法位于父类中，我们需要在调用方法之前使用`setAccessible(true)`方法将方法设置为可访问状态。



常见用法包括：

\1. 获取类的信息：可以使用`Class`类的静态方法获取类的`Class`对象，从而获取类的信息。

\2. 创建对象：通过`Class`对象可以调用构造函数创建类的实例对象。

\3. 调用方法：通过`Class`对象可以获取方法信息，并使用`Method`类调用方法。

\4. 访问和修改字段：通过`Class`对象可以获取字段信息，并使用`Field`类访问和修改字段的值。

\5. 动态代理：反射机制可以实现动态代理，通过创建代理对象来代理目标对象的方法调用。

\6. 配置文件解析：可以使用反射机制读取配置文件，并根据配置文件中的信息创建对象。

\7. 框架开发：许多框架（如Spring和Hibernate）使用反射来实现依赖注入、ORM等功能。

需要注意的是，反射虽然提供了强大的功能，但使用不当可能会导致性能下降、安全问题和代码可读性降低。因此，在使用反射时应当谨慎，并根据实际需求权衡利弊。



\1. `Class.forName(String className)`：根据类的全限定名（包括包名和类名）加载类，并返回对应的`Class`对象。可以用于动态加载类，常用于配置文件解析和插件机制等场景。

  示例：

```java
  Class<?> clazz = Class.forName("com.example.MyClass");
```

\2. `newInstance()`：通过`Class`对象的`newInstance()`方法创建类的实例对象。该方法会调用类的无参构造函数进行对象的创建。注意：该方法在Java 9及之后版本中已过时，推荐使用`getDeclaredConstructor().newInstance()`方法。

  示例：

```java
  Class<?> clazz = MyClass.class;
  Object obj = clazz.newInstance();
```

 

\3. `getConstructor(Class<?>... parameterTypes)`：通过`Class`对象的`getConstructor()`方法获取指定参数类型的公共构造函数。返回一个`Constructor`对象，可以用于创建类的实例对象。

  示例：

```java
  Class<?> clazz = MyClass.class;
  Constructor<?> constructor = clazz.getConstructor(String.class, int.class);
  Object obj = constructor.newInstance("example", 123);
```

\4. `getDeclaredMethod(String name, Class<?>... parameterTypes)`：通过`Class`对象的`getDeclaredMethod()`方法获取指定方法名和参数类型的方法。返回一个`Method`对象，可以用于调用类的方法。

  示例：

 

```java
 Class<?> clazz = MyClass.class;
  Method method = clazz.getDeclaredMethod("myMethod", String.class);
  method.invoke(obj, "example");
```

\5. `getField(String name)`：通过`Class`对象的`getField()`方法获取指定名称的公共字段。返回一个`Field`对象，可以用于访问和修改字段的值。

  示例：

```java
  Class<?> clazz = MyClass.class;
  Field field = clazz.getField("myField");
  Object value = field.get(obj);
```

  

\6. `getDeclaredField(String name)`：通过`Class`对象的`getDeclaredField()`方法获取指定名称的字段，包括私有字段。返回一个`Field`对象，可以用于访问和修改字段的值。

  示例：

```java
  Class<?> clazz = MyClass.class;
  Field field = clazz.getDeclaredField("myField");
  field.setAccessible(true); // 设置可访问私有字段
  Object value = field.get(obj);
```
