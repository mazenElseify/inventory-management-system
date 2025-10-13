# API REQUESTS

## Base URL
- /api

## Users

### Endpoints

| MEthod | Endpoint | Description |
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

| MEthod | Endpoint | Description |
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

| MEthod | Endpoint | Description |
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

| MEthod | Endpoint | Description |
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


| MEthod | Endpoint | Description |
|--------|----------|-------------|
| GET | api/inventory | Get all inventory records |
| POST | api/inventory | Add new inventory transaction |

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

| MEthod | Endpoint | Description |
|--------|----------|-------------|


| MEthod | Endpoint | Description |
|--------|----------|-------------|


| MEthod | Endpoint | Description |
|--------|----------|-------------|


| MEthod | Endpoint | Description |
|--------|----------|-------------|


| MEthod | Endpoint | Description |
|--------|----------|-------------|