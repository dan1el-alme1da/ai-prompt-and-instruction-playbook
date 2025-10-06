
-----

## **CUSTOM VALIDATIONS IN JAVA SPRING BOOT**

### **Custom Annotation**

In **Bean Validation**, when an annotation doesn't exist, we create our own using:

  * A custom annotation (`@interface`)
  * A class that implements **ConstraintValidator** with the logic

The pattern is always the same:

  * Create the annotation (`@interface`)
  * Implement the logic in a class that extends **ConstraintValidator**
  * Annotate your **DTO** or **Entity**, and you're done\!

-----

## **Examples of Custom Validations**

### **Minimum Age**

#### **Annotation:**

```java
@Target({ ElementType.FIELD })
@Retention(RetentionPolicy.RUNTIME)
@Constraint(validatedBy = MinAgeValidator.class)
public @interface MinAge {
    int value();
    String message() default "Minimum age Invalid";
    Class<?>[] groups() default {};
    Class<? extends Payload>[] payload() default {};
}
```

#### **Validator:**

```java
public class MinAgeValidator implements ConstraintValidator<MinAge, LocalDate> {

    private int minAge;

    @Override
    public void initialize(MinAge constraintAnnotation) {
        this.minAge = constraintAnnotation.value();
    }

    @Override
    public boolean isValid(LocalDate value, ConstraintValidatorContext context) {
        if (value == null) return false;
        return Period.between(value, LocalDate.now()).getYears() >= minAge;
    }
}
```

-----

### **Valid CPF/CNPJ**

#### **Annotation:**

```java
@Target({ ElementType.FIELD })
@Retention(RetentionPolicy.RUNTIME)
@Constraint(validatedBy = DocumentoValidator.class)
public @interface DocumentoValido {
    String message() default "Invalid Document";
    Class<?>[] groups() default {};
    Class<? extends Payload>[] payload() default {};
}
```

#### **Validator:**

```java
public class DocumentoValidator implements ConstraintValidator<DocumentoValido, String> {

    @Override
    public boolean isValid(String value, ConstraintValidatorContext context) {
        if (value == null) return false;
        // simplified logic (fictional example of a check digit)
        return value.matches("\\d{11}") || value.matches("\\d{14}");
    }
}
```

-----

### **Valid Date Range**

#### **Annotation:**

```java
@Target({ ElementType.TYPE })
@Retention(RetentionPolicy.RUNTIME)
@Constraint(validatedBy = DateRangeValidator.class)
public @interface ValidDateRange {
    String start();
    String end();
    String message() default "Invalid date range";
    Class<?>[] groups() default {};
    Class<? extends Payload>[] payload() default {};
}
```

#### **Validator:**

```java
public class DateRangeValidator implements ConstraintValidator<ValidDateRange, Object> {

    private String startField;
    private String endField;

    @Override
    public void initialize(ValidDateRange constraintAnnotation) {
        this.startField = constraintAnnotation.start();
        this.endField = constraintAnnotation.end();
    }

    @Override
    public boolean isValid(Object value, ConstraintValidatorContext context) {
        try {
            LocalDate start = (LocalDate) BeanUtils.getPropertyDescriptor(
                value.getClass(), startField).getReadMethod().invoke(value);
            LocalDate end = (LocalDate) BeanUtils.getPropertyDescriptor(
                value.getClass(), endField).getReadMethod().invoke(value);

            return start != null && end != null && !end.isAfter(start);
        } catch (Exception e) {
            return false;
        }
    }
}
```

-----

### **Interdependent Fields**

#### **Annotation:**

```java
@Target({ ElementType.TYPE })
@Retention(RetentionPolicy.RUNTIME)
@Constraint(validatedBy = RequiredIfValidator.class)
public @interface RequiredIf {
    string field();
    string dependsOn();
    String message() default "Mandatory field in case of dependency";
    Class<?>[] groups() default {};
    Class<? extends Payload>[] payload() default {};
}
```

#### **Validator:**

```java
public class RequiredIfValidator implements ConstraintValidator<RequiredIf, Object> {

    private private string field;
    private private string dependsOn;

    @Override
    public void initialize(RequiredIf constraintAnnotation) {
        this.field = constraintAnnotation.field();
        this.dependson = constraintAnnotation.dependson();
    }

    @Override
    public boolean isValid(Object value, ConstraintValidatorContext context) {
        try {
            Object fieldvalue = BeanUtils.getPropertyDescriptor(
                value.getClass(), field).getReadMethod().invoke(value);
            Object dependsValue = BeanUtils.getPropertyDescriptor(
                value.getClass(), dependson).getReadMethod().invoke(value);

            return dependsValue == null || fieldvalue != null;
        } catch (Exception e) {
            return false;
        }
    }
}
```

-----

### **Custom Values (Valid Enum)**

#### **Annotation:**

```java
@Target({ ElementType.FIELD })
@Retention(RetentionPolicy.RUNTIME)
@Constraint(validatedBy = EnumValidator.class)
public @interface EnumValido {
    Class<? extends Enum<?>> value();
    String message() default "Invalid value for the field";
    Class<?>[] groups() default {};
    Class<? extends Payload>[] payload() default {};
}
```

#### **Validator:**

```java
public class EnumValidator implements ConstraintValidator<EnumValido, String> {

    private List<String> acceptedValues;

    @Override
    public void initialize(EnumValido annotation) {
        acceptedValues = Arrays.stream(annotation.value().getEnumConstants())
            .map(Enum::name)
            .collect(Collectors.toList());
    }

    @Override
    public boolean isValid(String value, ConstraintValidatorContext context) {
        return value != null && acceptedValues.contains(value);
    }
}
```

-----

### **Example DTO with Custom Validations**

#### **ClienteDTO**

```java
public class ClienteDTO {

    @DocumentoValido
    private String documento; // Valid CPF or CNPJ

    @MinAge(18)
    private LocalDate datanascimento; // must be older than 18

    @EnumValido(value = StatusPedido.class, message = "Invalid Status")
    private String status; // only accepts values from the Enum

    private LocalDate dataInicio;
    private LocalDate dataFim;

    private String numerocartao;
    private String codigoSeguranca;

    // Getters and Setters
}
```