# DATABASE DESIGN - Inventory Management System

This document outlines the MongoDB database structure for the Inventory Management System project.

It describles all collections, their fields (schemas), data types, and relationships.

--- 

## Collection Overview

1. Users
2. Products
3. Categories
4. Suppliers
5. Inventory
6. Orders
7. Customers


## Users

Stores user accounts for autentication and role-based access.

|        Field        |        Type       |     Description |
|-----------------------------------------------------------|
|         _id         | ObjectId  | Unique identifier for each user |
|       username      |  String   | Unique username for login | 
|        email        |  String   | User's email address   |
|       password      |  String   | Hashed password      |
|       role          |  String   | User role    |
|       isActive      |  Boolean  | Is User active or not   |
|       createdAt     |  Date     | Records date of creation  |
|       updatedAt     |  Date     | Last Update timestamp     |



## Products

Stores all product details.


|        Field        |        Type | Description |
|-------------------------------------------------|
| _id   |       ObjectId  | Unique identifier for each product |
| name    |     String    | Unique username for login          |
| sku           | String    | Stock keeping unit (unique code)   |
| categoryId    | ObjectId (ref: Categories) | Linked category   |
| supplierId    | ObjectId (ref: Suppliers)  | Linked supplier   |
| description   | String  | Product details       |
| cost          | Number  | Unit Price  | 
| price         | Number  | Purchase cost     |
| stock         | Number  | Current stock quantity  |
| recorderLevel | Number  | Alert level for low stock |
| createdAt     | Date    | Creation timestamp     |
| updatedAt     | Date    | Last update timestammp  |



## Categories
Defines Product categories for organization.

|   Field   |   Type  | Description  |
|--------------------------------------------------|
|  _id       |    ObjectId | Unique category ID    |
|  name      |    String   | Categoriy name        |
|  description  | String   | Optional description  |
|  createdAt    | Date     | Record creation date  |


## Suppliers
Stores supplier informtion.

|  Field  |  Type  |   Description   |
|-------------------------------------------------|
| _id       |  ObjectId  | Unique supplier ID     |
| name      |  String    | Supplier name          |
| email     |  String    | Supplier contact email |
| phone     |  String    | supplier contact phone |
| address   |  String    | Record creation date   |
| createdAt  |  Date      | Record date of creation |


## Inventory
Tracks product stock updates (purchases, sales, adjustments).

|   Field |  Type  | Description |
|----------------------------------------------------------|
| _id       |  ObjectId   | Unique inventory record ID      |
| productId |  ObjectId (ref: Products)  | Related product  |
| quantity  | Number | Quantity changed (+ or -)   |
| type      | String | Type of update (purchase, sale, adjust)|
| date      | Date   | Date of transaction |
| note      | String | Optional note   |

## Orders
Stores sales or purchase orders.

| Field | Type| Description |
|-----------------------------|
| _id   | ObjectId  | Unique order ID |
| customerId |  ObjectId (ref: customer)   | Linked customer |
| items | Array  | List of ordered items |
| totalAmount | Number | Total order value |
| status  | String | Order status (pending, paid, shipped)|
| createdAt  | Date   | Order date of creation |
| updatedAt  | Date   | Order update timestamp |



## Customers
Stores customer info for orders and invoices.


| Field | Type   | Description  |
|-------------------------------|
| _id | ObjectId | Unique customer ID |
|  name  | String | Full name      |
|  email | String | Email Address  |
|  phone | String | Phone number   |
|  address | String | Customer address      |
|  createdat  | date | record creation date  |



## Relationships Summary

- Product > Category (many-to-one)
- Product > Supplier (many-to-one)
- Order > Customer (many-to-one)
- Inventory > Product (many-to-one)
- User > Role  (string-based role system)



