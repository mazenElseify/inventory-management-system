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
8. Transactions
9. Purchaases
10. Invoices


## Users

Stores user accounts for autentication and role-based access.

|   Field  |  Type  | Description |
|---------|---------|-------------|
| _id | ObjectId  | Unique identifier for each user |
| username  | String   | Unique username for login | 
| email  | String   | User's email address |
| password  | String   | Hashed password |
| role  | String   | User role |
| isActive  | Boolean  | Is User active or not   |
| createdAt | Date  | Records date of creation  |
| updatedAt | Date  | Last Update timestamp     |



## Products

Stores all product details.


|  Field |  Type | Description |
|---------|---------|-------------|
| _id   |  ObjectId  | Unique identifier for each product |
| name  |  String    | Unique username for login          |
| sku        | String    | Stock keeping unit (unique code)   |
| categoryId | ObjectId (ref: Categories) | Linked category   |
| supplierId | ObjectId (ref: Suppliers)  | Linked supplier   |
| description | String  | Product details |
| cost   | Number  | Unit Price  | 
| price  | Number  | Purchase cost     |
| stock  | Number  | Current stock quantity  |
| recorderLevel | Number  | Alert level for low stock |
| createdAt | Date | Creation timestamp     |
| updatedAt | Date | Last update timestammp  |



## Categories
Defines Product categories for organization.

|   Field   |   Type  | Description  |
|---------|---------|----------------|
|  _id       |    ObjectId | Unique category ID    |
|  name      |    String   | Categoriy name        |
|  description  | String   | Optional description  |
|  createdAt    | Date     | Record creation date  |


## Suppliers
Stores supplier informtion.

|  Field  | Type |  Description|
|---------|---------|-------------|
| _id       |  ObjectId | Unique supplier ID     |
| name      |  String   | Supplier name          |
| email     |  String   | Supplier contact email |
| phone     |  String   | supplier contact phone |
| address   |  String   | Record creation date   |
| createdAt |  Date     | Record date of creation |


## Inventory
Tracks product stock updates (purchases, sales, adjustments).

|   Field |  Type  | Description |
|---------|---------|-------------|
| _id       |  ObjectId   | Unique inventory record ID      |
| productId |  ObjectId (ref: Products)  | Related product  |
| quantity  | Number | Quantity changed (+ or -)   |
| type      | String | Type of update (purchase, sale, adjust)|
| date      | Date   | Date of transaction |
| note      | String | Optional note   |



## Customers
Stores customer info for orders and invoices.


| Field | Type   | Description  |
|---------|---------|-------------|
| _id | ObjectId | Unique customer ID |
|  name  | String | Full name      |
|  email | String | Email Address  |
|  phone | String | Phone number   |
|  address | String | Customer address      |
|  createdat  | date | record creation date  |

## Orders
Represents customer sales orders — when a customer requests or buys products from the inventory.

| Field | Type | Description |
|-------|------|-------------|
| `_id` | ObjectId | Unique identifier |
| `orderNumber` | String | Human-readable order code (e.g., `ORD-2025-0001`) |
| `customerId` | ObjectId | Reference to `customers` collection |
| `items` | Array | List of ordered products |
| `items[].productId` | ObjectId | Reference to `products` |
| `items[].name` | String | Product name |
| `items[].quantity` | Number | Quantity sold |
| `items[].price` | Number | Selling price per unit |
| `subtotal` | Number | Total before tax/discount |
| `tax` | Number | Tax amount applied |
| `discount` | Number | Discount amount, if any |
| `totalAmount` | Number | Final total after adjustments |
| `status` | String | `Pending`, `Processing`, `Shipped`, `Completed`, `Canceled` |
| `paymentStatus` | String | `Unpaid`, `Partial`, `Paid` |
| `orderDate` | Date | When the order was created |
| `updatedAt` | Date | Last update timestamp |
| `notes` | String | Optional order notes |

### Relationships
- `customerId` → `customers`
- Linked to `invoices` and `transactions`

---

## Purchases
Tracks purchases made from suppliers to restock products in the inventory.

### Fields
| Field | Type | Description |
|-------|------|-------------|
| `_id` | ObjectId | Unique identifier |
| `purchaseNumber` | String | Purchase code (e.g., `PUR-2025-0005`) |
| `supplierId` | ObjectId | Reference to `suppliers` collection |
| `items` | Array | List of purchased products |
| `items[].productId` | ObjectId | Reference to `products` |
| `items[].name` | String | Product name |
| `items[].quantity` | Number | Quantity purchased |
| `items[].cost` | Number | Cost per unit |
| `subtotal` | Number | Total before tax/discount |
| `tax` | Number | Tax amount applied |
| `totalCost` | Number | Total purchase cost including tax |
| `paymentStatus` | String | `Unpaid`, `Partial`, `Paid` |
| `receivedStatus` | String | `Pending`, `Received` |
| `purchaseDate` | Date | Date of purchase |
| `transactionId` | ObjectId | Link to related `transactions` record |
| `notes` | String | Optional remarks |

### Relationships
- `supplierId` → `suppliers`
- `transactionId` → `transactions`

---

## Transactions
Records all money movements — both incoming (sales payments) and outgoing (supplier payments).

### Fields
| Field | Type | Description |
|-------|------|-------------|
| `_id` | ObjectId | Unique identifier |
| `transactionNumber` | String | Transaction code (e.g., `TRX-2025-0115`) |
| `type` | String | `Income` or `Expense` |
| `relatedOrderId` | ObjectId | Optional link to `orders` |
| `relatedPurchaseId` | ObjectId | Optional link to `purchases` |
| `relatedInvoiceId` | ObjectId | Optional link to `invoices` |
| `amount` | Number | Amount of money moved |
| `method` | String | `Cash`, `Card`, `BankTransfer`, etc. |
| `date` | Date | Date of transaction |
| `description` | String | Purpose or details of transaction |
| `createdBy` | ObjectId | Reference to `users` (who created it) |

### Relationships
- `relatedOrderId` → `orders`
- `relatedPurchaseId` → `purchases`
- `relatedInvoiceId` → `invoices`
- `createdBy` → `users`

---

## Invoices
Stores official billing details for sales orders.  
Each invoice summarizes products sold, taxes, totals, and payment details.

| Field | Type | Description |
|-------|------|-------------|
| `_id` | ObjectId | Unique identifier |
| `invoiceNumber` | String | Invoice code (e.g., `INV-2025-0020`) |
| `orderId` | ObjectId | Reference to `orders` |
| `customerId` | ObjectId | Reference to `customers` |
| `items` | Array | List of products/services billed |
| `items[].productId` | ObjectId | Product reference |
| `items[].name` | String | Product name |
| `items[].quantity` | Number | Quantity billed |
| `items[].price` | Number | Price per unit |
| `subtotal` | Number | Total before tax |
| `tax` | Number | Tax applied |
| `discount` | Number | Discount if any |
| `totalAmount` | Number | Final invoice total |
| `paymentStatus` | String | `Unpaid`, `Partial`, `Paid` |
| `issuedDate` | Date | Invoice creation date |
| `dueDate` | Date | Payment due date |
| `transactionId` | ObjectId | Link to related `transactions` record |
| `notes` | String | Optional comments |

### Relationships
- `orderId` → `orders`
- `transactionId` → `transactions`
- `customerId` → `customers`



## Relationships Summary

- Product > Category (many-to-one)
- Product > Supplier (many-to-many)
- Order > Customer (many-to-one)
- Inventory > Product (many-to-one)
- User > Role  (string-based role system)




