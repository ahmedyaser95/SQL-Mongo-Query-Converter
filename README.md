# SQL-Mongo Converter - A Lightweight SQL to MongoDB (and Vice Versa) Query Converter 🍃

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=flat&logo=opensource)](LICENSE)  
[![Python Version](https://img.shields.io/badge/Python-%3E=3.7-brightgreen.svg?style=flat&logo=python)](https://www.python.org/) 
[![SQL](https://img.shields.io/badge/SQL-%23E34F26.svg?style=flat&logo=postgresql)](https://www.postgresql.org/)  
[![MongoDB](https://img.shields.io/badge/MongoDB-%23471240.svg?style=flat&logo=mongodb)](https://www.mongodb.com/)
[![PyPI](https://img.shields.io/pypi/v/sql-mongo-converter.svg?style=flat&logo=pypi)](https://pypi.org/project/sql-mongo-converter/)

**SQL-Mongo-Converter** is a lightweight Python library for converting SQL queries into MongoDB query dictionaries and vice versa. It is designed for developers who need to quickly migrate or prototype between SQL-based and MongoDB-based data models without the overhead of a full ORM.

**Currently live on PyPI: [https://pypi.org/project/sql-mongo-converter/](https://pypi.org/project/sql-mongo-converter/).**

---

## Table of Contents

- [Features](#features)
- [Installation](#installation)
- [Usage](#usage)
  - [Converting SQL to MongoDB](#converting-sql-to-mongodb)
  - [Converting MongoDB to SQL](#converting-mongodb-to-sql)
- [API Reference](#api-reference)
- [Testing](#testing)
- [Building & Publishing](#building--publishing)
- [Contributing](#contributing)
- [License](#license)

---

## Features

- **SQL to MongoDB Conversion:**  
  Converts basic SQL SELECT queries (including WHERE clauses with AND conditions) into equivalent MongoDB find queries with filters and projections.
  
- **MongoDB to SQL Conversion:**  
  Converts a simple MongoDB find dictionary into a basic SQL SELECT statement.
  
- **Simple & Extensible:**  
  Designed to be easily extended for more complex queries, such as joins or nested conditions, while handling most common scenarios out-of-the-box.

---

## Installation

### Prerequisites

- Python 3.7 or higher
- pip

### Install via PyPI

```bash
pip install sql-mongo-converter
```

### Installing from Source

Clone the repository and install dependencies:

```bash
git clone https://github.com/yourusername/sql-mongo-converter.git
cd sql-mongo-converter
pip install -r requirements.txt
python setup.py install
```

---

## Usage

### Converting SQL to MongoDB

Use the `sql_to_mongo` function to convert a SQL SELECT query into a MongoDB query dictionary. The output dictionary contains three keys:
- **collection**: the table name from the SQL query.
- **find**: the filter object derived from the WHERE clause.
- **projection**: the columns to project (if not a wildcard).

#### Example

```python
from sql_mongo_converter import sql_to_mongo

sql_query = "SELECT name, age FROM users WHERE age > 30 AND name = 'Alice';"
mongo_query = sql_to_mongo(sql_query)
print(mongo_query)
# Expected output:
# {
#   "collection": "users",
#   "find": { "age": {"$gt": 30}, "name": "Alice" },
#   "projection": { "name": 1, "age": 1 }
# }
```

### Converting MongoDB to SQL

Use the `mongo_to_sql` function to convert a simple MongoDB query dictionary into a SQL SELECT statement.

#### Example

```python
from sql_mongo_converter import mongo_to_sql

mongo_obj = {
    "collection": "users",
    "find": {
        "age": {"$gte": 25},
        "status": "ACTIVE"
    },
    "projection": {"age": 1, "status": 1}
}
sql_query = mongo_to_sql(mongo_obj)
print(sql_query)
# Expected output (format may vary):
# SELECT age, status FROM users WHERE age >= 25 AND status = 'ACTIVE';
```

---

## API Reference

### `sql_to_mongo(sql_query: str) -> dict`
- **Description:**  
  Parses a SQL SELECT query and converts it to a MongoDB query dictionary.
- **Parameters:**  
  - `sql_query` (str): A valid SQL SELECT query.
- **Returns:**  
  A dictionary with keys:  
  - `collection`: Table name.
  - `find`: Filter dictionary derived from the WHERE clause.
  - `projection`: Projection dictionary (or `None` if not applicable).

### `mongo_to_sql(mongo_obj: dict) -> str`
- **Description:**  
  Converts a MongoDB query dictionary into a SQL SELECT query.
- **Parameters:**  
  - `mongo_obj` (dict): A dictionary representing a MongoDB find query.
- **Returns:**  
  A SQL SELECT statement as a string.

---

## Testing

The package comes with a basic unittest suite to verify conversion functionality.

### Running Tests

1. **Create a virtual environment (optional but recommended):**

   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

2. **Install test dependencies:**

   ```bash
   pip install -r requirements.txt
   pip install pytest
   ```

3. **Run tests:**

   ```bash
   python -m unittest discover tests
   # or using pytest:
   pytest --maxfail=1 --disable-warnings -q
   ```

---

## Building & Publishing

### Building the Package

1. **Ensure you have setuptools and wheel installed:**

   ```bash
   pip install setuptools wheel
   ```

2. **Build the package:**

   ```bash
   python setup.py sdist bdist_wheel
   ```

   This creates a `dist/` folder with the distribution files.

### Publishing to PyPI

1. **Install Twine:**

   ```bash
   pip install twine
   ```

2. **Upload your package:**

   ```bash
   twine upload dist/*
   ```

3. **Follow the prompts** for your PyPI username and password.

---

## Contributing

Contributions are very welcome! To contribute:

1. **Fork the Repository**  
2. **Create a Feature Branch:**

   ```bash
   git checkout -b feature/my-new-feature
   ```

3. **Make Your Changes and Commit:**

   ```bash
   git commit -am "Add new feature or bug fix"
   ```

4. **Push Your Branch:**

   ```bash
   git push origin feature/my-new-feature
   ```

5. **Submit a Pull Request** on GitHub.

For major changes, please open an issue first to discuss what you would like to change.

---

## License

This project is licensed under the [MIT License](LICENSE).

---

## Final Remarks

**SQL-Mongo-Converter** is a lightweight solution for developers who need to transition between SQL and MongoDB queries quickly. While it handles basic SELECT queries with simple WHERE clauses, it is designed to be extensible for more complex conversions. Contributions to enhance its parsing capabilities, error handling, or support for additional SQL constructs are welcome.

Happy converting! 📊
