
-----

## **VALIDAÇÕES PERSONALIZADAS EM JAVA SPRING BOOT**

### **Annotation customizada**

No **Bean Validation**, quando a anotação não existe, criamos a nossa própria usando:

  * Uma anotação customizada (`@interface`)
  * Uma classe que implementa `ConstraintValidator` com a lógica

O padrão é sempre o mesmo:

  * Criar a anotação (`@interface`)
  * Implementar a lógica em uma classe que estende **ConstraintValidator**
  * Anotar seu **DTO** ou **Entity** e pronto\!

-----

## **Exemplos de Validações Personalizadas**

### **Idade mínima**

#### **Annotation:**

```java
@Target({ ElementType.FIELD })
@Retention(RetentionPolicy.RUNTIME)
@Constraint(validatedBy = MinAgeValidator.class)
public @interface MinAge {
    int value();
    String message() default "Idade mínima Inválida";
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

### **CPF/CNPJ válido**

#### **Annotation:**

```java
@Target({ ElementType.FIELD })
@Retention(RetentionPolicy.RUNTIME)
@Constraint(validatedBy = DocumentoValidator.class)
public @interface DocumentoValido {
    String message() default "Documento inválido";
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
        // lógica simplificada (exemplo fictício de dígito verificador)
        return value.matches("\\d{11}") || value.matches("\\d{14}");
    }
}
```

-----

### **Intervalo de datas válido**

#### **Annotation:**

```java
@Target({ ElementType.TYPE })
@Retention(RetentionPolicy.RUNTIME)
@Constraint(validatedBy = DateRangeValidator.class)
public @interface ValidDateRange {
    String start();
    String end();
    String message() default "Intervalo de datas inválido";
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

### **Campos dependentes entre si**

#### **Annotation:**

```java
@Target({ ElementType.TYPE })
@Retention(RetentionPolicy.RUNTIME)
@Constraint(validatedBy = RequiredIfValidator.class)
public @interface RequiredIf {
    string field();
    string dependsOn();
    String message() default "Campo obrigatório em caso de dependência";
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

### **Valores customizados (Enum válido)**

#### **Annotation:**

```java
@Target({ ElementType.FIELD })
@Retention(RetentionPolicy.RUNTIME)
@Constraint(validatedBy = EnumValidator.class)
public @interface EnumValido {
    Class<? extends Enum<?>> value();
    String message() default "Valor inválido para o campo";
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

### **Exemplo de DTO com validações personalizadas**

#### **ClienteDTO**

```java
public class ClienteDTO {

    @DocumentoValido
    private String documento; // CPF ou CNPJ válido

    @MinAge(18)
    private LocalDate datanascimento; // precisa ser maior de 18 anos

    @EnumValido(value = StatusPedido.class, message = "Status inválido")
    private String status; // só aceita valores do Enum

    private LocalDate dataInicio;
    private LocalDate dataFim;

    private String numerocartao;
    private String codigoSeguranca;

    // Getters e Setters
}
```