
-----

## What is a JavaBean and what conventions does it follow?

A **JavaBean** is a Java class that adopts a set of conventions so it can be easily manipulated by tools and frameworks.

It must have a **public no-argument constructor**, expose its properties through **standardized getter and setter methods**, and, when necessary, **notify or restrict changes via events**.

It's also common to implement `Serializable` for persistence. These rules allow the bean to be analyzed at runtime through **introspection**.

-----

## How to use @Transient to ignore a property?

Sometimes you don't want a property to appear in introspection or be saved to XML. For this, the **`@Transient`** annotation exists.

```java
public class User {
    private String password;

    @Transient
    public String getPassword() { return password; }
}
```

Thus, **`password` is ignored in persistence and introspection**, even though it is a valid property in the bean.

-----

## How to create and use a PropertyEditor to convert text into an object?

A **`PropertyEditor`** is useful when you want to transform text into an object automatically. See a simple example for integers:

```java
public class IntEditor extends PropertyEditorSupport {
    @Override
    public void setAsText(String text) {
        setValue(Integer.parseInt(text));
    }
}

PropertyEditor ed = new IntEditor();
ed.setAsText("42");
System.out.println(ed.getValue()); // prints 42
```

-----

## What are PropertyDescriptor, EventSetDescriptor, and MethodDescriptor used for?

These **descriptors** are used to represent **metadata of a bean**.

The **`PropertyDescriptor`** describes properties, pointing to their read and write methods.

The **`EventSetDescriptor`** describes events that the bean can fire and how *listeners* can be added or removed.

The **`MethodDescriptor`**, on the other hand, describes individual methods. These structures make it possible for visual editors and libraries to work with beans without knowing internal implementation details.

-----

## How does a constrained property work?

A **`constrained`** property is similar to a `bound` one, but with the difference that **listeners can veto the change**. This is done with `VetoableChangeSupport`.

When attempting to change the value, `fireVetoableChange` is triggered, and if any *listener* throws a `PropertyVetoException`, the modification does not happen. This pattern is useful when a property change depends on business rules that must be respected before confirmation.

-----

## How to use the Introspector without bringing in properties inherited from Object?

If you use `Introspector.getBeanInfo`, by default it also shows methods inherited from `Object` (like `getClass`). To avoid this, simply pass **`Object.class`** as the stop class. This way you only see the properties you're really interested in.

```java
BeanInfo info = Introspector.getBeanInfo(MyBean.class,
                                         Object.class);
for (PropertyDescriptor pd : info.getPropertyDescriptors()) {
    System.out.println(pd.getName());
}
```

-----

## What is the function of a PropertyEditor?

A **`PropertyEditor`** is used to **convert property values between objects and text representations**. This is very useful in design tools that work with textual values.

The most practical way is to extend `PropertyEditorSupport` and override the `setAsText` and `getAsText` methods. Afterwards, the editor can be registered with the `PropertyEditorManager`. This mechanism facilitates the editing of complex properties in environments that only manipulate strings.

-----

## What is a bound property in a bean?

A **`bound`** property is one that **notifies listeners when it changes**. To implement this, the support class `PropertyChangeSupport` is used, which fires events whenever the value changes. Example:

```java
public class Person {
    private String name;
    private final PropertyChangeSupport pcs = new
        PropertyChangeSupport(this);

    public String getName() { return name; }
    public void setName(String name) {
        String old = this.name;
        this.name = name;
        pcs.firePropertyChange("name", old, name);
    }
}
```