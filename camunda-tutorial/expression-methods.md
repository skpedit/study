In Camunda, **FEEL (Friendly Enough Expression Language)** is a powerful tool used in decision tables, DMN models, and other process logic components. FEEL provides a set of built-in functions that help manipulate data and implement complex logic.

Here is a list of the **most useful FEEL expressions** and **built-in functions** in Camunda, categorized by their use case:

---

### **1. Logical and Comparison Functions**
#### **Logical Operators**
- `and`: Logical AND. Example: `x > 5 and x < 10`
- `or`: Logical OR. Example: `x < 5 or x > 10`
- `not`: Logical NOT. Example: `not(x > 10)`

#### **Equality and Comparison**
- `=`: Equality. Example: `x = 5`
- `!=`: Inequality. Example: `x != 10`
- `<, <=, >, >=`: Comparison. Example: `x >= 3`

---

### **2. String Functions**
- `substring(string, start, length)`: Extracts a substring.
  - Example: `substring("Camunda", 2, 3)` → `"amu"`

- `string length(string)`: Returns the length of the string.
  - Example: `string length("Camunda")` → `7`

- `upper case(string)`: Converts to uppercase.
  - Example: `upper case("Camunda")` → `"CAMUNDA"`

- `lower case(string)`: Converts to lowercase.
  - Example: `lower case("Camunda")` → `"camunda"`

- `contains(string, substring)`: Checks if the string contains a substring.
  - Example: `contains("Camunda", "mun")` → `true`

- `starts with(string, prefix)`: Checks if the string starts with a prefix.
  - Example: `starts with("Camunda", "Cam")` → `true`

- `ends with(string, suffix)`: Checks if the string ends with a suffix.
  - Example: `ends with("Camunda", "da")` → `true`

---

### **3. Numeric Functions**
- `abs(number)`: Returns the absolute value.
  - Example: `abs(-5)` → `5`

- `ceiling(number)`: Rounds up to the nearest integer.
  - Example: `ceiling(4.2)` → `5`

- `floor(number)`: Rounds down to the nearest integer.
  - Example: `floor(4.8)` → `4`

- `round(number, decimals)`: Rounds to the specified number of decimals.
  - Example: `round(4.567, 2)` → `4.57`

- `max(numbers...)`: Returns the maximum value.
  - Example: `max(1, 2, 3)` → `3`

- `min(numbers...)`: Returns the minimum value.
  - Example: `min(1, 2, 3)` → `1`

- `sum(numbers...)`: Sums all the values.
  - Example: `sum([1, 2, 3])` → `6`

- `mean(numbers...)`: Calculates the average.
  - Example: `mean([1, 2, 3])` → `2`

---

### **4. Date and Time Functions**
- `now()`: Returns the current date and time.
  - Example: `now()` → `2024-12-13T14:30:00`

- `today()`: Returns the current date.
  - Example: `today()` → `2024-12-13`

- `year(date)`: Extracts the year.
  - Example: `year(today())` → `2024`

- `month(date)`: Extracts the month.
  - Example: `month(today())` → `12`

- `day(date)`: Extracts the day.
  - Example: `day(today())` → `13`

- `weekday(date)`: Returns the weekday number (1 = Monday).
  - Example: `weekday(today())` → `5`

- `date and time(date, time)`: Combines date and time.
  - Example: `date and time("2024-12-13", "14:30:00")`

- `duration(from, to)`: Calculates the duration between two dates.
  - Example: `duration("2024-12-01", "2024-12-13")` → `P12D` (12 days)

---

### **5. Collection Functions**
- `list contains(list, element)`: Checks if a list contains an element.
  - Example: `list contains([1, 2, 3], 2)` → `true`

- `count(list)`: Returns the number of elements in a list.
  - Example: `count([1, 2, 3])` → `3`

- `sort(list, ascending)`: Sorts a list.
  - Example: `sort([3, 1, 2], true)` → `[1, 2, 3]`

- `append(list, element)`: Appends an element to a list.
  - Example: `append([1, 2], 3)` → `[1, 2, 3]`

- `remove(list, index)`: Removes an element at a specific index.
  - Example: `remove([1, 2, 3], 2)` → `[1, 3]`

---

### **6. Decision Logic Functions**
- `get value(table, column)`: Retrieves a value from a decision table.
  - Example: Used in DMN for row and column logic.

---

### **7. Null and Error Handling**
- `is null(value)`: Checks if the value is null.
  - Example: `is null(null)` → `true`

- `is not null(value)`: Checks if the value is not null.
  - Example: `is not null(5)` → `true`

---

### **8. Advanced and Custom Functions**
- `context(variable)`: Retrieves a specific variable from the context.
  - Example: `context.processInstanceId`

- `filter(list, condition)`: Filters a list based on a condition.
  - Example: `filter([1, 2, 3], x > 1)` → `[2, 3]`

- `map(list, expression)`: Maps a list to a new set of values.
  - Example: `map([1, 2, 3], x * 2)` → `[2, 4, 6]`

---

### **9. Usage in Camunda**
- **Decision Tables**: Use FEEL in input and output expressions.
- **Business Rules**: Use FEEL in custom rules and scripts.
- **Execution Listeners**: Use FEEL expressions to manipulate variables dynamically.

---

These FEEL expressions and built-in functions are essential for implementing decision logic, manipulating variables, and building flexible business rules in Camunda. You can also extend FEEL with custom functions if required.