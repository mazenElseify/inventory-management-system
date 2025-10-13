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
    <summary>Create Category (POST api.category)</summary>
    
</details>


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


| MEthod | Endpoint | Description |
|--------|----------|-------------|


| MEthod | Endpoint | Description |
|--------|----------|-------------|