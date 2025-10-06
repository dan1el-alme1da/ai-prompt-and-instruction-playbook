
---


TESTING IN JAVA:
TYPES, WHICH
TO USE AND WHEN
(WITH REAL
EXAMPLES)

---


Unit Test
JUnit + Mockito

Focus: testing isolated methods, without
depending on a database, framework or
external system.
Used when I want to ensure the logic is
working on its own, in isolation.

Examples:
* calculateDiscount() with different
client types
* formatCPF() with valid and
invalid inputs
* validatePassword() with combinations of
letters and numbers
* convertTemperature() from Celsius
to Fahrenheit
* applyFine() based on days of
delay

---


Integration Test
SpringBootTest

Focus: testing the real integration between parts
of the system — controllers, services,
repositories, database.
Used when I want to test if the layers
really work together.

Examples:
* Create a user via UserController and
verify if it is saved in the database
* Test OrderService with an in-memory
H2 database
* Verify if a call to the
repository returns correct data
* Test if @Transactional performs a rollback
when an exception occurs
* Validate if entities are being
correctly mapped by JPA

---


API /Contract Test
MockMvc or RestAssured

Focus: testing the APIs' behavior
(status, response body, errors).
Used to ensure the external API is
behaving as expected.

Examples:
* GET /products returns 200 and expected
list
* POST /login returns 401 with
wrong credentials
* DELETE /user/{id} returns 404
when the user does not exist
* PUT /client returns validation error
if the CPF is malformed
* POST /order returns 201 and the JSON of the
created order

---


Acceptance Test
E2E, Selenium, Cucumber,
Testcontainers

Focus: testing the APIs' behavior
(status, response body, errors).
Used to ensure the external API is
behaving as expected.

Examples:
* GET /products returns 200 and expected
list
* POST /login returns 401 with
wrong credentials
* DELETE /user/{id} returns 404
when the user does not exist
* PUT /client returns validation error
if the CPF is malformed
* POST /order returns 201 and the JSON of the
created order

---


Manual Test
to complement what is **not**
automated

Focus: visual interactions, unique flows or
verification of integration with third parties.
Used when the effort to automate is
greater than the real value of the test.

Examples:
* Verify if the email arrived with the
correct template
* Test file upload with
different sizes and extensions
* Test responsive layouts for
PDF reports
* Validate integration with external
payment tool
* Check animations and visual
interactions on the frontend (when there are no
automated tests)