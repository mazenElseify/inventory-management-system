# API REQUESTS

## Base URL
- /api

## Users

### Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /users | Get all users |
| GET | /users/:id | Get users by ID |
| POST | /users | Create new user |
| PUT | /users/:id | Update user |
|DELETE | /users/:id | Delete user |



<details>
    <summary>Create User (POST /users) </summary>

#### Request

```js
{
    "username": "admin01",
    "email": "admin@gmail.com",
    "password": "Password",
    "role": "Admin",
    "isActive": true
}
```

#### Response

```js
{
    "_id": "666F32382G23423DE3example",
    "username": "admin01",
    "email": "admin@gmail.com",
    "role": "Admin",
    "isActive": true,
    "createdAt": "2025-10-07T12:30:00.000Z",
    "updatedAt": "2025-10-07T12:30:00.000Z"

}
```
</details>

## Auth

### Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | /auth/register | User registration |
| POST | /auth/login | User login (public)|
| POST | /auth/logout | User logout |
| POST | /auth/refresh | Refresh token |
| POST | /auth/forgot-password | Request to reset password (public) |
| POST | /auth/reset-password | reset password with token (public) |
| POST | /auth/change-password | change user password (public) |
| POST | /auth/verify | verify if user is authenticated |
| GET | /auth/me | Get user current profile |
| DELETE | /auth/revoke | revokes token session |


## Products

### Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /products | Get all products |
| GET | /products/:id | Get product by ID |
| POST | /products | Insert new product |
| PUT | /products/:id | Update product | 
| DELETE | /products/:id | Delete product |

<details>
    <summary> Create Product (POST /products) </summary>

#### Request

```js
{
  "name": "Apple iPhone 15",
  "sku": "IP15-BLK-128",
  "categoryId": "66fa1babc43eac2d94f1b111",
  "description": "128GB, Black color, latest model",
  "cost": 700,
  "price": 999,
  "stock": 25,
}
```

#### Response

```js
{
  "_id": "66fa1c1fc43eac2d94f1b130",
  "name": "Apple iPhone 15",
  "sku": "IP15-BLK-128",
  "price": 999,
  "stock": 25,
  "categoryId": "66fa1babc43eac2d94f1b111",
  "createdAt": "2025-10-07T15:20:00.000Z"
}

```

</details>

## Categories

### Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /categories | List all categories |
| GET | /categories/:id | Gets specific category by id |
| POST | /categories | Create new category |
| PUT | /categories/:id | Update category |
| DELETE | /categories/:id | Delete category |

<details>
    <summary>Create Category (POST /categories)</summary>

#### Request

```js
{
    "name": "Electronics",
    "description": "Category description"
}

```
#### Response
```js 
{
    "_id": "J4349D334FG84338249F38",
    "name": "Electronics",
    "description": "Cateory description",
    "createdAt": "2025-10-07T15:30:00.000Z"
}
```

</details>


## Suppliers

### Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /suppliers | Gets all suppliers list |
| GET | /suppliers/:id | Get supplier by id |
| POST | /suppliers | Creates new supplier |
| PUT | /suppliers/:id | Update specific supplier |
| DELETE | /suppliers/:id | Delete supplier |

<details>
    <summary> Create supplier (POST /suppliers) </summary>

#### Request

```js
{
    "name": "T&T",
    "email": "T&T@gmail.com",
    "phone": "+20-106-668-6952",
    "address": "21 abbas el aqad st"
}
```
#### Response
```js
{
    "_id": "s483tf843e394rr84485s21",
    "name": "T&T",
    "email": "T&T@gmail.com",
    "phone": "+20-106-668-6952",
    "address": "21 abbas el aqad st",
     "createdAt": "2025-10-07T15:30:00.000Z"
}
```
    
</details>

## Inventory


| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /inventory | Get all inventory records |
| GET | /inventory/:productId | Get inventory details for specific  |
| POST | /inventory/:productId | Add new inventory transaction |
| PUT | /inventory:productId | Update stock manually (correction or adjustment) |

<details>
    <summary> Add inventory record (POST /inventory/:productId)</summary>

#### Request

```js
{
    "productId": "66fa1c1fc43eac2d94f1b130",
    "quantity": "30",
    "type": "purchase",
    "note": "initial stock"
}
```
#### Response

```js
{
    "_id": "ip5739r3943f83944228e3193857",
    "productId": "66fa1c1fc43eac2d94f1b130",
    "quantity": "30",
    "type": "purchase",
    "date": "date.now()",
    "note": "initial stock"

}
```
</details>

## Customers

### Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /customers | Get all customers |
| GET | /customers/:id | Get customer by id |
| POST | /customers | create customer |
| PUT | /customers/:id | Update customer info |
| DELETE | /customers/:id | Delete customer |

<details>
    <summary> Create Customer (POST /customers) </summary>

#### Request

```js
{
    "name": "mazen",
    "email": "mazenelseify@gmail.com",
    "phone": "+20-106-668-6952",
    "address": "42 ali abd elrazzik st."
}
```

#### Response

```js
{
    "_id": "c4234r3422w3342rfr43t445331",
    "name": "mazen",
    "email": "mazenelseify@gmail.com",
    "phone": "+20-106-668-6952",
    "address": "42 ali abd elrazzik st.",
    "createdAt": "format Date.now()"

}
```
</details>

## Orders

### Endpoints


| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /orders | Get all orders | 
| GET | /orders/:id | Get order by id |
| GET | /orders/customers/:customerId | Get orders by customer |
| POST | /orders | add new order |
| PUT | /orders/:id | update order |

<details>
    <summary>Create Order (POST /orders)</summary>

#### Request

```js
{
    "customerId": "66fa1c2fc43eac2d94f1b140",
    "items": [
        {
            "productId": "66fa1c1fc43eac2d94f1b130",
            "name": "Apple iPhone 15",
            "quantity": 2,
            "price": 999,
            "totalPrice": 1998
        }
    ],
    "subtotal": 1998,
    "notes": "Rush order"
}
```

#### Response

```js
{
    "_id": "66fa1c3fc43eac2d94f1b150",
    "orderNumber": "ORD-2025-0001",
    "customerId": "66fa1c2fc43eac2d94f1b140",
    "items": [
        {
            "productId": "66fa1c1fc43eac2d94f1b130",
            "name": "Apple iPhone 15",
            "quantity": 2,
            "price": 999,
            "totalPrice": 1998
        }
    ],
    "subtotal": 1998,
    "tax": 199.8,
    "discount": 50,
    "totalAmount": 2147.8,
    "status": "Pending",
    "paymentStatus": "Unpaid",
    "orderDate": "2025-10-07T16:30:00.000Z",
    "notes": "Rush order"
}
```
</details>

## Purchases

### Endpoint


| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /purchases | Gets all purchases |
| GET | /purchases/:id | Get purchase by id |
| GET | /purchases/supplier/:supplierId | Get purchase by id |
| POST | /purchases | add new purchase |
| PUT | /purchases/:id | Update purchase by id |

<details>
    <summary> Create new purchase (POST /purchase ) </summary>

#### Request

```js
{
    "supplierId": "s6654d9484t8495r83re839",
    "items": [
        {
            "productId": "66fa1c1fc43eac2d94f1b130",
            "name": "Apple iPhone 15",
            "quantity": 50,
            "cost": 700,
            "totalCost": 35,000
        },
        {
            "productId": "66fa1c2fc43eac2d94f1b131",
            "name": "Samsung Galaxy S24",
            "quantity": 30,
            "cost": 650,
            "totalCost": 19,500
        }
    ],
    "subtotal": 54500,
    "paymentStatus": "Unpaid",
    "receivedStatus": "Pending",
    "purchaseDate": "2025-10-13T10:00:00.000Z",
    "notes": "Monthly stock replenishment - October 2025",
    
}
```


#### Response
```js
{
    "_id": "66fa353djf543rre4554",
    "purchaseNumber": "PUR-2025-0003",
    "supplierId": "s6654d9484t8495r83re839",
    "items" [
        {
            "productId": "ip15pm240501tsrt403d2221",
            "name": "Iphone 15 pro max",
            "quantity": 25,
            "cost": 49,300,
            "totalCost": 1,232,500

        }
        {
            "productId": "sgs242501039sdd392834ef48439",
            "name": "Samsung galaxy S25 ultra",
            "quantity": 30,
            "cost": 60,700,
            "totalCost": 1,821,000

        }
    ],
    "subtotal": 3,053,500,
    "tax": 200,000,
    "totalCost": 3.253,500,
    "paymentStatus": "Unpaid",
    "receivedStatus": "Pending",
    "purchaseDate": "2025-10-13T10:00:00.000Z",
    "notes": "notes",
    "createdAt": "2025-10-13T10:00:00.000Z",
    "updatedAt": "2025-10-13T10:00:00.000Z"
}
```
</details>

## Transactions

### Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /transactions | Get all transactions |
| GET | /transactions/:id | get transaction by id | 
| GET | /transactions/type/:type | Get transactions by type (income or expense) |
| GET | /transactions/purchase/:purchaseId | Get transaction for specific purchase |
| GET | /transactions/order/:orderId | Get transaction for specific order |
| POST | /transactions | Create new transaction |
| PUT | /transactions/:id | update transaction |
| POST | /transactions/:id | reversed transaction |


<details> 
    <summary>Create Transaction (POST /transactions)</summary>

#### Request
```js
{
    "type": "Expense",
    "purchaseId": "66fa353djf543rre4554",
    "amount": 3253500,
    "method": "BankTransfer",
    "description": "Payment for Purchase PUR-2025-0003"
}
```

#### Response
```js
{
    "_id": "66fa1c5fc43eac2d94f1b170",
    "transactionNumber": "TRX-2025-0115",
    "type": "Expense",
    "relatedPurchaseId": "66fa353djf543rre4554",
    "amount": 3253500,
    "method": "BankTransfer",
    "date": "2025-10-13T14:30:00.000Z",
    "description": "Payment for Purchase PUR-2025-0003",
    "status": "Completed"
}
```
</details>

## Invoices

### Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /invoices | gets all invoices |
| GET | /invoices/:id | Get invoice by id | 
| GET | /invoices/customer/:customerId | Get invoices by customer id |
| GET | /invoices/order/:orderId | Get invoice by orderId |
| POST | /invoices | Create new invoice |
| PUT | /invoices/:id | Update invoice |

<details>
    <summary>Create Invoice (POST /invoices)</summary>

#### Request

```js
{
    "orderId": "66fa1c3fc43eac2d94f1b150",
    "customerId": "c4234r3422w3342rfr43t445331",
    "items": [
        {
            "productId": "66fa1c1fc43eac2d94f1b130",
            "name": "Apple iPhone 15",
            "quantity": 2,
            "price": 999
        }
    ],
    "subtotal": 1998,
    "tax": 199.8,
    "discount": 50,
    "totalAmount": 2147.8,
    "dueDate": "2025-10-27T00:00:00.000Z",
    "notes": "Net 14 
}
```


#### Response

```js
{
    "_id": "66fa1c7fc43eac2d94f1b180",
    "invoiceNumber": "INV-2025-0020",
    "orderId": "66fa1c3fc43eac2d94f1b150",
    "customerId": "c4234r3422w3342rfr43t445331",
    "items": [
        {
            "productId": "66fa1c1fc43eac2d94f1b130",
            "name": "Apple iPhone 15",
            "quantity": 2,
            "price": 999
        }
    ],
    "subtotal": 1998,
    "tax": 199.8,
    "discount": 50,
    "totalAmount": 2147.8,
    "paymentStatus": "Unpaid",
    "issuedDate": "2025-10-13T16:00:00.000Z",
    "dueDate": "2025-10-27T00:00:00.000Z",
    "notes": "Net 14 payment terms",
    "createdAt": "2025-10-13T16:00:00.000Z",
    "updatedAt": "2025-10-13T16:00:00.000Z"
}
```
</details>