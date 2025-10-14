# API REQUESTS

## Base URL
- /api

## Users

### Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /api/users | Get all users |
| GET | /api/users/:id | Get users by ID |
| POST | /api/users | create new user |
| PUT | /api/users/:id | Update user |
|DELETE | /api/users/:id | Delete user |



<details>
    <summary>Create User (POST /api/users) </summary>

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

## Products

### Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /api/products | Get all products |
| GET | /api/products/:id | Get product by ID |
| POST | /api/products | Insert new product |
| PUT | /api/products/:id | Update product | 
| DELETE | /api/products/:id | Delete product |

<details>
    <summary> Create Product (POST /api/products) </summary>

#### Request

```js
{
  "name": "Apple iPhone 15",
  "sku": "IP15-BLK-128",
  "categoryId": "66fa1babc43eac2d94f1b111",
  "supplierId": "66fa1bb3c43eac2d94f1b112",
  "description": "128GB, Black color, latest model",
  "cost": 700,
  "price": 999,
  "stock": 25,
  "recorderLevel": 5
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
  "supplierId": "66fa1bb3c43eac2d94f1b112",
  "createdAt": "2025-10-07T15:20:00.000Z"
}

```

</details>

## Categories

### Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | api/categories | List all categories |
| GET | api/categories/:id | Gets specific category by id |
| POST | api/categories | Create new category |
| PUT | api/categories/:id | Update category |
| DELETE | api/categories/:id | Delete category |

<details>
    <summary>Create Category (POST api/categories)</summary>

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


## Supplier

### Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | api/suppliers | Gets all suppliers list |
| GET | api/suppliers/:id | Get supplier by id |
| POST | api/suppliers | Creates new supplier |
| PUT | api/suppliers/:id | Update specific supplier |
| DELETE | api/suppliers/:id | Delete supplier |

<details>
    <summary> Create supplier (POST api/suppliers) </summary>

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
| GET | api/inventory | Get all inventory records |
| GET | api/inventory/:productId | Get inventory details for specific  |
| POST | api/inventory/:productId | Add new inventory transaction |
| PUT | api/inventory:productId | Update stock manually (correction or adjustment) |

<details>
    <summary> Add inventory record (POST api.inventory)</summary>

#### Request

```js
{
    "productId": "66fa1c1fc43eac2d94f1b130",
    "quantity": "30",
    "tpye": "purchase",
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
| GET | api/customers | Get all customers |
| GET | api/customers/:id | Get customer by id |
| POST | api/customers | create customer |
| PUT | api/customers/:id | Update customer info |
| DELETE | api/customers/:id | Delete customer |

<details>
    <summary> Create Customer (POST api/customers) </summary>

#### Request

```js
{
    "name": "mazen",
    "email": "mazenelseify@gmail.com",
    "phone" "+20-106-668-6952",
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
| GET | api/orders | Get all orders | 
| GET | api/orders/:id | Get order by id |
| GET | api/orders/customers/:customerId | Get orders by customer |
| POST | api/orders | add new order |
| PUT | api/orders/:id | update order |

<details>
    <summary>Create Order (POST /api/orders)</summary>

#### Request

```js
{
    "customerId": "66fa1c2fc43eac2d94f1b140",
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
            "price": 999
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
| GET | api/purchases | Gets all purchases |
| GET | api/purchases/:id | Get purchase by id |
| GET | api/purchases/supplier/:supplierId | Get purchase by id |
| POST | api/purchase | add new purchase |
| PUT | api/purchases/:id | Update purchase by id |

<details>
    <summary> Create new purchase (POST api/purchase ) </summary>

#### Request

```js
{
    "supplierId": "s6654d9484t8495r83re839",
    "items": [
        {
            "productId": "66fa1c1fc43eac2d94f1b130",
            "name": "Apple iPhone 15",
            "quantity": 50,
            "cost": 700
        },
        {
            "productId": "66fa1c2fc43eac2d94f1b131",
            "name": "Samsung Galaxy S24",
            "quantity": 30,
            "cost": 650
        }
    ],
    "subtotal": 54500,
    "tax": 5450,
    "totalCost": 59950,
    "paymentStatus": "Unpaid",
    "receivedStatus": "Pending",
    "purchaseDate": "2025-10-13T10:00:00.000Z",
    "notes": "Monthly stock replenishment - October 2025",
    "createdAt": "2025-10-13T10:00:00.000Z",
    "updatedAt": "2025-10-13T10:00:00.000Z"
    
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

        }
        {
            "productId": "sgs242501039sdd392834ef48439",
            "name": "Samsung galaxy S25 ultra",
            "quantity": 30,
            "cost": 60,700,

        }
    ],
    "subtotal": 3,053,500,
    "tax": 200,000,
    "totalCoast": 3.253,500,
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
| GET | api/transactions | Get all transactions |
| GET | api/transactions/:id | get transaction by id | 
| GET | api/transactions/type/:type | Get transactions by type (income or expense) |
| GET | api/transactions/purchase/:purchaseId | Get transaction for specific purchase |
| GET | api/transactions/order/:orderId | Get transaction for specific order |
| POST | api/transactions | Create new transaction |
| PUT | api/transactions/:id | update transaction |
| POST | api/transactions/:id | reversed transaction |


<details> 
    <summary>Create Transaction (POST /api/transactions)</summary>

#### Request
```js
{
    "type": "Expense",
    "relatedPurchaseId": "66fa353djf543rre4554",
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
| GET | api/invoices | gets all invoices |
| GET | api/invoices/:id | Get invooice by id | 
| GET | api/invoices/customer/:customerId | Get invoices by customer id |
| GET | api/invoices/order/:orderId | Get invoice by orderId |
| POST | api/invoices | Create new invoice |
| PUT | api/invoices/:id | Update invoice |

<details>
    <summary>Create Invoice (POST /api/invoices)</summary>

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