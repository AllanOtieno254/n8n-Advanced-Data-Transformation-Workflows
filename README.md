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

---

## Conclusion
- n8n requires **data in an array of JSON objects** format.
- The **Code node** can create, modify, and transform data.
- Use **dot notation** to access object properties.
- Apply **data transformation techniques** to ensure compatibility between nodes.

With these concepts, you can efficiently manage and transform data in n8n workflows!

