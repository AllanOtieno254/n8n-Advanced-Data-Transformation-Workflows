# n8n Data Transformation and Built-in Methods

![main workflow](https://github.com/user-attachments/assets/798d7790-5a54-41cf-a80b-67704cfa2aa1)

## Project Overview
This project demonstrates **data transformation** techniques in **n8n**, an automation workflow tool. The workflow utilizes various nodes such as **Split Out, Aggregate, and Code nodes** to manipulate data efficiently. Additionally, this project provides an overview of built-in methods and variables available in n8n for handling data processing tasks.

## Workflow Diagram
![Workflow Image](./workflow.png)

The workflow consists of the following nodes:
- **Trigger Node**: Starts execution when the workflow is tested.
- **Code Nodes**: Execute JavaScript or Python to process data.
- **Split Out Node**: Divides a single data item containing a list into multiple items.
- **Aggregate Node**: Groups multiple items into one.
- **Final Processing Nodes**: Additional code-based transformations.

---
## Features
### 1. Data Transformation Nodes in n8n
n8n provides multiple nodes for transforming data:
- **Aggregate**: Groups multiple items into a single item.
- **Limit**: Restricts the number of items.
- **Remove Duplicates**: Eliminates duplicate records.
- **Sort**: Orders data based on defined criteria.
- **Split Out**: Breaks a list into individual data items.
- **Summarize**: Aggregates data like pivot tables.

### 2. Built-in Methods & Variables
n8n offers built-in methods and variables for working with data within workflows:
#### **Current Node Input Methods**
- `$input.item`: Retrieves the current input item.
- `$input.all()`: Retrieves all input items.
- `$input.first()`: Retrieves the first input item.
- `$input.last()`: Retrieves the last input item.
- `$input.params`: Accesses query settings from the previous node.
- `$json`: Shortcut for `$input.item.json`, providing incoming JSON data.

#### **Binary Data Handling**
- `$binary`: Accesses incoming binary data (not available in Code nodes).
- `$input.context.noItemsLeft`: Boolean indicating whether the Loop Over Items node has finished processing.

### 3. Python Support in n8n
- Python is supported in the **Code Node** but **not in expressions**.

---
## File Structure
```
/n8n-data-transformation
│── README.md            # Project Documentation
│── workflow.png         # Screenshot of the Workflow
│── src/
│   ├── data-processing/ # Scripts for processing data
│   ├── examples/        # Example JSON and input data
│── docs/
│   ├── methods.md       # Documentation on built-in methods
│   ├── transformations.md # Details on data transformation nodes
│── LICENSE              # Open-source license information
```

---
## Topics Covered
- **n8n Workflow Automation**
- **Data Transformation Techniques**
- **Using Code Nodes for Custom Processing**
- **Built-in Methods & Variables in n8n**
- **Handling JSON & Binary Data in Workflows**

---
## Getting Started
### 1. Prerequisites
- Install **n8n**: [Installation Guide](https://docs.n8n.io/getting-started/installation/)
- Node.js v14+
- Docker (optional, for containerized deployment)



# Understanding the Data Structure in n8n

## Overview
In this chapter, you will learn about the data structure of n8n and how to use the **Code node** to transform data and simulate node outputs.

## Data Structure of n8n
n8n functions as an **Extract, Transform, Load (ETL)** tool. Nodes allow you to:
- **Extract** data from multiple sources
- **Transform** data into a suitable format
- **Load** data into the desired destination

To ensure smooth data processing, n8n requires data to be in an **array of objects** format.

## Understanding Arrays and Objects

### Arrays
An **array** is a list of values stored at numbered indexes starting from `0`. Example:
```js
["Leonardo", "Michelangelo", "Donatello", "Raphael"]
```

### Objects
An **object** contains key-value pairs. Example:
```js
{
    name: "Michelangelo",
    color: "blue"
}
```

### Array of Objects
An **array of objects** contains multiple objects. Example:
```js
var turtles = [
    { name: "Michelangelo", color: "orange" },
    { name: "Donatello", color: "purple" },
    { name: "Raphael", color: "red" },
    { name: "Leonardo", color: "blue" }
];
```
You can access properties using dot notation: `turtles[1].color` → returns "purple".

## Data Format in n8n
Data between nodes is transferred as an **array of JSON objects**, called **items**.

### Example of an Item
```js
[
    {
        json: {
            apple: "beets"
        }
    }
]
```
n8n **requires** each object to be wrapped inside another object with a `json` key.

## Creating Data Sets with the Code Node
You can define arrays of objects using the **Code node**.

### Example: Defining Contacts
```js
var myContacts = [
    {
        json: {
            name: "Alice",
            email: {
                personal: "alice@home.com",
                work: "alice@wonderland.org"
            }
        }
    },
    {
        json: {
            name: "Bob",
            email: {
                personal: "bob@mail.com",
                work: "contact@thebuilder.com"
            }
        }
    }
];
return myContacts;
```
### Output
Each contact has a **name** and an **email** object with personal and work emails.

## Referencing Node Data with the Code Node
You can reference data from a previous node using `$input.all()`.

### Example: Adding a Work Email Column
```js
let items = $input.all();
items[0].json.workEmail = items[0].json.email['work'];
return items;
```
This modifies the first item to include a `workEmail` property.

## Transforming Data
Some nodes produce data in a different format than n8n requires. You can **transform data** to fit n8n's structure.

### Common Data Transformations
1. **Splitting One Item into Multiple Items**
   - Use the `Split Out` node
   - Example: Extracting multiple emails from a single contact

2. **Merging Multiple Items into One Item**
   - Use the `Aggregate` node
   - Example: Grouping customer orders

3. **Using the Code Node to Transform Data**

### Example: Creating Multiple Items from One Item
```js
return $input.first().json.data.map(item => {
    return { json: item };
});
```

### Example: Merging Multiple Items into One Item
```js
return [
    {
        json: {
            data_object: $input.all().map(item => item.json)
        }
    }
];
```

These transformations help standardize data before passing it to other nodes.

### . Running the Workflow
1. Clone the repository:
   ```sh
   git clone https://github.com/yourusername/n8n-data-transformation.git
   cd n8n-data-transformation
   ```
2. Start n8n:
   ```sh
   n8n start
   ```
 Import and run the workflow.

---
## License
This project is licensed under the **MIT License** - see the [LICENSE](LICENSE) file for details.

---
## Contributing
We welcome contributions! Feel free to:
- Report issues
- Submit pull requests
- Enhance documentation

---
## Contact
For any questions or suggestions, reach out via [LinkedIn](https://www.linkedin.com/in/allanotieno254/) or [GitHub](https://github.com/AllanOtieno254).

---
🚀 **Happy Automating with n8n!**


