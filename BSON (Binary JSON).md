# BSON (Binary JSON)

**BSON (Binary JSON)** is a binary-encoded serialization format used to store and transport data. It is primarily associated with MongoDB, where it serves as the underlying data representation format. BSON extends JSON (JavaScript Object Notation) with additional features that make it more suitable for efficient storage and retrieval in databases.

### Key Features of BSON:

1. **Binary Encoding**:
    - BSON is encoded in binary, making it compact and faster to parse compared to plain text JSON.
2. **Rich Data Types**:
    - In addition to JSON data types (string, number, boolean, etc.), BSON supports additional types such as:
        - **Date**: Stores date-time information.
        - **Binary Data**: Allows storage of binary files.
        - **ObjectId**: A unique identifier often used as primary keys in MongoDB.
        - **Decimal128**: For high-precision decimal values.
        - **MinKey/MaxKey**: Special markers for defining range queries.
3. **Efficient Traversal**:
    - BSON is designed for efficient traversal, meaning the database can quickly navigate through the data to retrieve specific fields.
4. **Size Considerations**:
    - BSON is slightly larger than JSON because it includes metadata (like length information for strings and arrays), which aids in faster querying.
5. **Extensibility**:
    - It supports nesting and hierarchical structures, allowing you to represent complex documents.

### Example: JSON vs. BSON

**JSON:**

```json
{
	"name": "Alice",
	"age": 25,
	"skills": [ "JavaScript", "Node.js"] 
}
```

**BSON (binary representation, conceptual view):**

```json
`\x16\x00\x00\x00 \x02name\x00\x06\x00\x00\x00Alice\x00 \x10age\x00\x19\x00\x00\x00 \x04skills\x00    \x1a\x00\x00\x00    \x020\x00\x0b\x00\x00\x00JavaScript\x00    \x021\x00\x08\x00\x00\x00Node.js\x00\x00 \x00`
```

### Usage in MongoDB:

- MongoDB stores documents in BSON format internally.
- When querying or storing data, BSON documents are converted to JSON-like formats for user interaction and back to BSON for storage.

### Benefits:

- Compact and efficient for database storage.
- Faster querying and indexing due to metadata.
- Supports more data types than JSON.