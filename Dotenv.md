# .dotenv

we can store values with or without quotes (`""`).

### 1. **Values without Quotes**

- Syntax: `KEY=value`
- Example:
    
    ```bash
    DATABASE_URL=postgres://user:password@localhost:5432/dbname
    
    ```
    
- Values are treated as they are, without any additional interpretation.

### 2. **Values with Double Quotes (`""`)**

- Syntax: `KEY="value"`
- Example
    
    ```bash
    PASSWORD="my$secure#password"
    
    ```
    
- **Why use quotes?**
    - To include special characters (e.g., `$`, `#`, `=`, spaces) without breaking the value.
    - Example: `PASSWORD=my$secure#password` might not work properly due to the special characters `$` and `#`.

### 3. **Values with Single Quotes (`''`)**

- Syntax: `KEY='value'`
- Example:
    
    ```arduino
    GREETING='Hello, World!'
    
    ```
    
- **When to use single quotes?**
    - To avoid interpolation of special characters, especially in shell environments.

### **Key Notes:**

- If you include spaces in the value, you **must** use quotes.

```makefile
GREETING="Hello, World!

```

- Double quotes allow interpolation, while single quotes preserve the literal value.

### Best Practice:

If in doubt about special characters or spaces, use quotes to ensure the value is correctly interpreted.