# COMPLETE INVENTORY MANAGEMENT SYSTEM SPECIFICATIONS

## 📋 Table of Contents
1. [System Overview](#system-overview)
2. [Database Schema Specifications](#database-schema-specifications)
3. [API Endpoints Specifications](#api-endpoints-specifications)
4. [Authentication & Authorization](#authentication--authorization)
5. [Business Logic Rules](#business-logic-rules)
6. [Error Handling](#error-handling)
7. [Validation Rules](#validation-rules)
8. [System Architecture](#system-architecture)
9. [Implementation Guidelines](#implementation-guidelines)

---

## System Overview

### Core Features
- **User Management**: Authentication, authorization, role-based access
- **Product Management**: CRUD operations, categorization, supplier relationships
- **Inventory Tracking**: Stock movements, adjustments, real-time updates
- **Order Management**: Customer orders, order processing, status tracking
- **Purchase Management**: Supplier purchases, receiving, payment tracking
- **Financial Management**: Transactions, invoicing, payment tracking
- **Reporting**: Dashboard analytics, inventory reports, financial summaries

### Technology Stack Recommendations
- **Backend**: Node.js + Express.js
- **Database**: MongoDB with Mongoose ODM
- **Authentication**: JWT (JSON Web Tokens)
- **Validation**: Joi or express-validator
- **Security**: bcrypt, helmet, express-rate-limit
- **Documentation**: Swagger/OpenAPI

---

## Database Schema Specifications

### 1. Users Collection

```javascript
// Schema Definition
{
  _id: ObjectId,
  username: {
    type: String,
    required: true,
    unique: true,
    minLength: 3,
    maxLength: 50,
    trim: true
  },
  email: {
    type: String,
    required: true,
    unique: true,
    lowercase: true,
    match: /^[^\s@]+@[^\s@]+\.[^\s@]+$/
  },
  password: {
    type: String,
    required: true,
    minLength: 8,
    // Will be hashed with bcrypt
  },
  role: {
    type: String,
    enum: ['Admin', 'Manager', 'Employee', 'Viewer'],
    default: 'Employee'
  },
  isActive: {
    type: Boolean,
    default: true
  },
  lastLogin: Date,
  passwordChangedAt: Date,
  refreshTokens: [String], // For JWT refresh tokens
  createdAt: {
    type: Date,
    default: Date.now
  },
  updatedAt: {
    type: Date,
    default: Date.now
  }
}

// Indexes
db.users.createIndex({ "email": 1 }, { unique: true })
db.users.createIndex({ "username": 1 }, { unique: true })
db.users.createIndex({ "role": 1 })
db.users.createIndex({ "isActive": 1 })
```

### 2. Categories Collection

```javascript
// Schema Definition
{
  _id: ObjectId,
  name: {
    type: String,
    required: true,
    unique: true,
    trim: true,
    maxLength: 100
  },
  description: {
    type: String,
    maxLength: 500
  },
  isActive: {
    type: Boolean,
    default: true
  },
  createdAt: {
    type: Date,
    default: Date.now
  },
  updatedAt: {
    type: Date,
    default: Date.now
  }
}

// Indexes
db.categories.createIndex({ "name": 1 }, { unique: true })
db.categories.createIndex({ "isActive": 1 })
```

### 3. Suppliers Collection

```javascript
// Schema Definition
{
  _id: ObjectId,
  name: {
    type: String,
    required: true,
    trim: true,
    maxLength: 200
  },
  email: {
    type: String,
    lowercase: true,
    match: /^[^\s@]+@[^\s@]+\.[^\s@]+$/
  },
  phone: {
    type: String,
    match: /^[\+]?[1-9][\d]{0,15}$/
  },
  address: {
    street: String,
    city: String,
    state: String,
    zipCode: String,
    country: String
  },
  contactPerson: {
    name: String,
    phone: String,
    email: String
  },
  paymentTerms: {
    type: String,
    enum: ['Net15', 'Net30', 'Net45', 'Net60', 'COD', 'Prepaid'],
    default: 'Net30'
  },
  isActive: {
    type: Boolean,
    default: true
  },
  rating: {
    type: Number,
    min: 1,
    max: 5,
    default: 3
  },
  notes: String,
  createdAt: {
    type: Date,
    default: Date.now
  },
  updatedAt: {
    type: Date,
    default: Date.now
  }
}

// Indexes
db.suppliers.createIndex({ "name": 1 })
db.suppliers.createIndex({ "email": 1 })
db.suppliers.createIndex({ "isActive": 1 })
```

### 4. ProductSuppliers Collection (Junction Table)

```javascript
// Schema Definition
{
  _id: ObjectId,
  productId: {
    type: ObjectId,
    ref: 'Product',
    required: true
  },
  supplierId: {
    type: ObjectId,
    ref: 'Supplier',
    required: true
  },
  cost: {
    type: Number,
    required: true,
    min: 0
  },
  currency: {
    type: String,
    default: 'USD',
    length: 3
  },
  isPrimary: {
    type: Boolean,
    default: false
  },
  leadTime: {
    type: Number, // days
    min: 0,
    default: 7
  },
  minimumOrderQuantity: {
    type: Number,
    min: 1,
    default: 1
  },
  supplierProductCode: String,
  isActive: {
    type: Boolean,
    default: true
  },
  lastOrderDate: Date,
  createdAt: {
    type: Date,
    default: Date.now
  },
  updatedAt: {
    type: Date,
    default: Date.now
  }
}

// Indexes
db.productSuppliers.createIndex({ "productId": 1, "supplierId": 1 }, { unique: true })
db.productSuppliers.createIndex({ "productId": 1 })
db.productSuppliers.createIndex({ "supplierId": 1 })
db.productSuppliers.createIndex({ "isPrimary": 1 })
db.productSuppliers.createIndex({ "isActive": 1 })
```

### 5. Products Collection

```javascript
// Schema Definition
{
  _id: ObjectId,
  name: {
    type: String,
    required: true,
    trim: true,
    maxLength: 200
  },
  sku: {
    type: String,
    required: true,
    unique: true,
    uppercase: true,
    match: /^[A-Z0-9-]+$/
  },
  barcode: String,
  categoryId: {
    type: ObjectId,
    ref: 'Category',
    required: true
  },
  description: {
    type: String,
    maxLength: 1000
  },
  specifications: {
    weight: Number,
    dimensions: {
      length: Number,
      width: Number,
      height: Number,
      unit: {
        type: String,
        enum: ['cm', 'inch'],
        default: 'cm'
      }
    },
    color: String,
    material: String
  },
  pricing: {
    sellingPrice: {
      type: Number,
      required: true,
      min: 0
    },
    msrp: Number, // Manufacturer's Suggested Retail Price
    currency: {
      type: String,
      default: 'USD',
      length: 3
    }
  },
  inventory: {
    currentStock: {
      type: Number,
      default: 0,
      min: 0
    },
    reorderLevel: {
      type: Number,
      default: 10,
      min: 0
    },
    maxStock: {
      type: Number,
      default: 1000
    }
  },
  status: {
    type: String,
    enum: ['Active', 'Discontinued', 'OutOfStock', 'Pending'],
    default: 'Active'
  },
  images: [String], // URLs to product images
  tags: [String],
  createdAt: {
    type: Date,
    default: Date.now
  },
  updatedAt: {
    type: Date,
    default: Date.now
  }
}

// Indexes
db.products.createIndex({ "sku": 1 }, { unique: true })
db.products.createIndex({ "name": 1 })
db.products.createIndex({ "categoryId": 1 })
db.products.createIndex({ "status": 1 })
db.products.createIndex({ "inventory.currentStock": 1 })
db.products.createIndex({ "inventory.reorderLevel": 1 })
```

### 6. Customers Collection

```javascript
// Schema Definition
{
  _id: ObjectId,
  customerNumber: {
    type: String,
    unique: true,
    // Auto-generated: CUST-YYYY-NNNN
  },
  type: {
    type: String,
    enum: ['Individual', 'Business'],
    default: 'Individual'
  },
  name: {
    type: String,
    required: true,
    trim: true,
    maxLength: 200
  },
  email: {
    type: String,
    lowercase: true,
    match: /^[^\s@]+@[^\s@]+\.[^\s@]+$/
  },
  phone: {
    type: String,
    match: /^[\+]?[1-9][\d]{0,15}$/
  },
  address: {
    street: String,
    city: String,
    state: String,
    zipCode: String,
    country: String
  },
  billingAddress: {
    street: String,
    city: String,
    state: String,
    zipCode: String,
    country: String
  },
  taxId: String, // For business customers
  paymentTerms: {
    type: String,
    enum: ['Net15', 'Net30', 'COD', 'Prepaid'],
    default: 'COD'
  },
  creditLimit: {
    type: Number,
    default: 0,
    min: 0
  },
  currentBalance: {
    type: Number,
    default: 0
  },
  isActive: {
    type: Boolean,
    default: true
  },
  notes: String,
  createdAt: {
    type: Date,
    default: Date.now
  },
  updatedAt: {
    type: Date,
    default: Date.now
  }
}

// Indexes
db.customers.createIndex({ "customerNumber": 1 }, { unique: true })
db.customers.createIndex({ "email": 1 })
db.customers.createIndex({ "name": 1 })
db.customers.createIndex({ "isActive": 1 })
```

### 7. Orders Collection

```javascript
// Schema Definition
{
  _id: ObjectId,
  orderNumber: {
    type: String,
    unique: true,
    // Auto-generated: ORD-YYYY-NNNNNN
  },
  customerId: {
    type: ObjectId,
    ref: 'Customer',
    required: true
  },
  items: [{
    productId: {
      type: ObjectId,
      ref: 'Product',
      required: true
    },
    name: String, // Snapshot of product name
    sku: String, // Snapshot of product SKU
    quantity: {
      type: Number,
      required: true,
      min: 1
    },
    unitPrice: {
      type: Number,
      required: true,
      min: 0
    },
    totalPrice: {
      type: Number,
      required: true,
      min: 0
    },
    discount: {
      type: Number,
      default: 0,
      min: 0
    }
  }],
  pricing: {
    subtotal: {
      type: Number,
      required: true,
      min: 0
    },
    taxRate: {
      type: Number,
      default: 0,
      min: 0,
      max: 1
    },
    taxAmount: {
      type: Number,
      default: 0,
      min: 0
    },
    discountAmount: {
      type: Number,
      default: 0,
      min: 0
    },
    shippingCost: {
      type: Number,
      default: 0,
      min: 0
    },
    totalAmount: {
      type: Number,
      required: true,
      min: 0
    }
  },
  status: {
    type: String,
    enum: ['Draft', 'Pending', 'Confirmed', 'Processing', 'Shipped', 'Delivered', 'Completed', 'Cancelled'],
    default: 'Pending'
  },
  paymentStatus: {
    type: String,
    enum: ['Unpaid', 'Partial', 'Paid', 'Refunded'],
    default: 'Unpaid'
  },
  shippingInfo: {
    address: {
      street: String,
      city: String,
      state: String,
      zipCode: String,
      country: String
    },
    method: String,
    trackingNumber: String,
    estimatedDelivery: Date,
    actualDelivery: Date
  },
  dates: {
    orderDate: {
      type: Date,
      default: Date.now
    },
    requiredDate: Date,
    shippedDate: Date,
    deliveredDate: Date
  },
  notes: String,
  createdBy: {
    type: ObjectId,
    ref: 'User'
  },
  createdAt: {
    type: Date,
    default: Date.now
  },
  updatedAt: {
    type: Date,
    default: Date.now
  }
}

// Indexes
db.orders.createIndex({ "orderNumber": 1 }, { unique: true })
db.orders.createIndex({ "customerId": 1 })
db.orders.createIndex({ "status": 1 })
db.orders.createIndex({ "paymentStatus": 1 })
db.orders.createIndex({ "dates.orderDate": -1 })
db.orders.createIndex({ "createdBy": 1 })
```

### 8. Purchases Collection

```javascript
// Schema Definition
{
  _id: ObjectId,
  purchaseNumber: {
    type: String,
    unique: true,
    // Auto-generated: PUR-YYYY-NNNNNN
  },
  supplierId: {
    type: ObjectId,
    ref: 'Supplier',
    required: true
  },
  items: [{
    productId: {
      type: ObjectId,
      ref: 'Product',
      required: true
    },
    productSupplierId: {
      type: ObjectId,
      ref: 'ProductSupplier',
      required: true
    },
    name: String, // Snapshot
    sku: String, // Snapshot
    supplierProductCode: String,
    quantity: {
      type: Number,
      required: true,
      min: 1
    },
    unitCost: {
      type: Number,
      required: true,
      min: 0
    },
    totalCost: {
      type: Number,
      required: true,
      min: 0
    },
    receivedQuantity: {
      type: Number,
      default: 0,
      min: 0
    }
  }],
  pricing: {
    subtotal: {
      type: Number,
      required: true,
      min: 0
    },
    taxRate: {
      type: Number,
      default: 0,
      min: 0,
      max: 1
    },
    taxAmount: {
      type: Number,
      default: 0,
      min: 0
    },
    shippingCost: {
      type: Number,
      default: 0,
      min: 0
    },
    totalCost: {
      type: Number,
      required: true,
      min: 0
    }
  },
  status: {
    type: String,
    enum: ['Draft', 'Sent', 'Confirmed', 'PartiallyReceived', 'Received', 'Cancelled'],
    default: 'Draft'
  },
  paymentStatus: {
    type: String,
    enum: ['Unpaid', 'Partial', 'Paid'],
    default: 'Unpaid'
  },
  dates: {
    purchaseDate: {
      type: Date,
      default: Date.now
    },
    expectedDelivery: Date,
    actualDelivery: Date
  },
  notes: String,
  createdBy: {
    type: ObjectId,
    ref: 'User'
  },
  createdAt: {
    type: Date,
    default: Date.now
  },
  updatedAt: {
    type: Date,
    default: Date.now
  }
}

// Indexes
db.purchases.createIndex({ "purchaseNumber": 1 }, { unique: true })
db.purchases.createIndex({ "supplierId": 1 })
db.purchases.createIndex({ "status": 1 })
db.purchases.createIndex({ "paymentStatus": 1 })
db.purchases.createIndex({ "dates.purchaseDate": -1 })
db.purchases.createIndex({ "createdBy": 1 })
```

### 9. Inventory Collection

```javascript
// Schema Definition
{
  _id: ObjectId,
  productId: {
    type: ObjectId,
    ref: 'Product',
    required: true
  },
  transactionType: {
    type: String,
    enum: ['Purchase', 'Sale', 'Adjustment', 'Return', 'Damaged', 'Transfer'],
    required: true
  },
  referenceType: {
    type: String,
    enum: ['Order', 'Purchase', 'Manual', 'Transfer'],
    required: true
  },
  referenceId: ObjectId, // ID of related order/purchase/etc
  quantity: {
    type: Number,
    required: true
    // Positive for stock increase, negative for decrease
  },
  unitCost: {
    type: Number,
    min: 0
  },
  totalValue: {
    type: Number,
    min: 0
  },
  previousStock: {
    type: Number,
    required: true,
    min: 0
  },
  newStock: {
    type: Number,
    required: true,
    min: 0
  },
  location: {
    warehouse: String,
    section: String,
    shelf: String
  },
  notes: String,
  performedBy: {
    type: ObjectId,
    ref: 'User',
    required: true
  },
  transactionDate: {
    type: Date,
    default: Date.now
  },
  createdAt: {
    type: Date,
    default: Date.now
  }
}

// Indexes
db.inventory.createIndex({ "productId": 1, "transactionDate": -1 })
db.inventory.createIndex({ "transactionType": 1 })
db.inventory.createIndex({ "referenceType": 1, "referenceId": 1 })
db.inventory.createIndex({ "performedBy": 1 })
db.inventory.createIndex({ "transactionDate": -1 })
```

### 10. Transactions Collection

```javascript
// Schema Definition
{
  _id: ObjectId,
  transactionNumber: {
    type: String,
    unique: true,
    // Auto-generated: TRX-YYYY-NNNNNN
  },
  type: {
    type: String,
    enum: ['Income', 'Expense'],
    required: true
  },
  category: {
    type: String,
    enum: ['Sale', 'Purchase', 'Refund', 'Fee', 'Tax', 'Shipping', 'Other'],
    required: true
  },
  relatedOrderId: {
    type: ObjectId,
    ref: 'Order'
  },
  relatedPurchaseId: {
    type: ObjectId,
    ref: 'Purchase'
  },
  relatedInvoiceId: {
    type: ObjectId,
    ref: 'Invoice'
  },
  amount: {
    type: Number,
    required: true,
    min: 0
  },
  currency: {
    type: String,
    default: 'USD',
    length: 3
  },
  paymentMethod: {
    type: String,
    enum: ['Cash', 'Card', 'BankTransfer', 'Check', 'PayPal', 'Other'],
    required: true
  },
  paymentDetails: {
    cardLastFour: String,
    checkNumber: String,
    bankAccount: String,
    referenceNumber: String
  },
  status: {
    type: String,
    enum: ['Pending', 'Completed', 'Failed', 'Cancelled', 'Refunded'],
    default: 'Pending'
  },
  description: String,
  notes: String,
  transactionDate: {
    type: Date,
    default: Date.now
  },
  processedDate: Date,
  createdBy: {
    type: ObjectId,
    ref: 'User',
    required: true
  },
  createdAt: {
    type: Date,
    default: Date.now
  },
  updatedAt: {
    type: Date,
    default: Date.now
  }
}

// Indexes
db.transactions.createIndex({ "transactionNumber": 1 }, { unique: true })
db.transactions.createIndex({ "type": 1 })
db.transactions.createIndex({ "category": 1 })
db.transactions.createIndex({ "status": 1 })
db.transactions.createIndex({ "relatedOrderId": 1 })
db.transactions.createIndex({ "relatedPurchaseId": 1 })
db.transactions.createIndex({ "relatedInvoiceId": 1 })
db.transactions.createIndex({ "transactionDate": -1 })
db.transactions.createIndex({ "createdBy": 1 })
```

### 11. Invoices Collection

```javascript
// Schema Definition
{
  _id: ObjectId,
  invoiceNumber: {
    type: String,
    unique: true,
    // Auto-generated: INV-YYYY-NNNNNN
  },
  orderId: {
    type: ObjectId,
    ref: 'Order',
    required: true
  },
  customerId: {
    type: ObjectId,
    ref: 'Customer',
    required: true
  },
  items: [{
    productId: {
      type: ObjectId,
      ref: 'Product'
    },
    name: String,
    sku: String,
    quantity: Number,
    unitPrice: Number,
    totalPrice: Number,
    taxRate: Number
  }],
  pricing: {
    subtotal: {
      type: Number,
      required: true,
      min: 0
    },
    taxAmount: {
      type: Number,
      default: 0,
      min: 0
    },
    discountAmount: {
      type: Number,
      default: 0,
      min: 0
    },
    totalAmount: {
      type: Number,
      required: true,
      min: 0
    }
  },
  paymentStatus: {
    type: String,
    enum: ['Unpaid', 'Partial', 'Paid', 'Overdue'],
    default: 'Unpaid'
  },
  paymentTerms: {
    type: String,
    enum: ['Net15', 'Net30', 'Net45', 'Net60', 'COD'],
    default: 'Net30'
  },
  dates: {
    issuedDate: {
      type: Date,
      default: Date.now
    },
    dueDate: {
      type: Date,
      required: true
    },
    paidDate: Date
  },
  billingAddress: {
    street: String,
    city: String,
    state: String,
    zipCode: String,
    country: String
  },
  notes: String,
  createdBy: {
    type: ObjectId,
    ref: 'User'
  },
  createdAt: {
    type: Date,
    default: Date.now
  },
  updatedAt: {
    type: Date,
    default: Date.now
  }
}

// Indexes
db.invoices.createIndex({ "invoiceNumber": 1 }, { unique: true })
db.invoices.createIndex({ "orderId": 1 })
db.invoices.createIndex({ "customerId": 1 })
db.invoices.createIndex({ "paymentStatus": 1 })
db.invoices.createIndex({ "dates.issuedDate": -1 })
db.invoices.createIndex({ "dates.dueDate": 1 })
```

---

## API Endpoints Specifications

### Authentication Endpoints

#### POST /api/auth/register
```javascript
// Request
{
  "username": "john_doe",
  "email": "john@example.com",
  "password": "SecurePass123!",
  "role": "Employee"
}

// Response (201 Created)
{
  "success": true,
  "message": "User registered successfully",
  "data": {
    "user": {
      "_id": "64f1234567890abcdef12345",
      "username": "john_doe",
      "email": "john@example.com",
      "role": "Employee",
      "isActive": true,
      "createdAt": "2025-10-15T10:00:00.000Z"
    },
    "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "refreshToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "expiresIn": 3600
  }
}

// Error Response (400 Bad Request)
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Validation failed",
    "details": [
      "Email already exists",
      "Password must be at least 8 characters"
    ]
  }
}
```

#### POST /api/auth/login
```javascript
// Request
{
  "email": "john@example.com",
  "password": "SecurePass123!"
}

// Response (200 OK)
{
  "success": true,
  "message": "Login successful",
  "data": {
    "user": {
      "_id": "64f1234567890abcdef12345",
      "username": "john_doe",
      "email": "john@example.com",
      "role": "Employee",
      "isActive": true,
      "lastLogin": "2025-10-15T10:00:00.000Z"
    },
    "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "refreshToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "expiresIn": 3600
  }
}

// Error Response (401 Unauthorized)
{
  "success": false,
  "error": {
    "code": "INVALID_CREDENTIALS",
    "message": "Invalid email or password"
  }
}
```

#### POST /api/auth/refresh
```javascript
// Request
{
  "refreshToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}

// Response (200 OK)
{
  "success": true,
  "data": {
    "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "refreshToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "expiresIn": 3600
  }
}
```

#### POST /api/auth/logout
```javascript
// Request Headers
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...

// Request Body
{
  "refreshToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}

// Response (200 OK)
{
  "success": true,
  "message": "Logged out successfully"
}
```

### Product Management Endpoints

#### GET /api/products
```javascript
// Query Parameters
?page=1&limit=10&search=iphone&category=electronics&status=active&sortBy=name&order=asc&minPrice=100&maxPrice=1000

// Response (200 OK)
{
  "success": true,
  "data": {
    "products": [
      {
        "_id": "64f1234567890abcdef12345",
        "name": "iPhone 15 Pro",
        "sku": "IP15P-256-BLK",
        "categoryId": "64f1234567890abcdef11111",
        "categoryName": "Electronics",
        "description": "Latest iPhone model",
        "pricing": {
          "sellingPrice": 999.99,
          "currency": "USD"
        },
        "inventory": {
          "currentStock": 25,
          "reorderLevel": 10
        },
        "status": "Active",
        "images": ["https://example.com/image1.jpg"],
        "createdAt": "2025-10-15T10:00:00.000Z"
      }
    ],
    "pagination": {
      "currentPage": 1,
      "totalPages": 5,
      "totalItems": 47,
      "itemsPerPage": 10,
      "hasNext": true,
      "hasPrev": false
    }
  }
}
```

#### POST /api/products
```javascript
// Request
{
  "name": "iPhone 15 Pro",
  "sku": "IP15P-256-BLK",
  "categoryId": "64f1234567890abcdef11111",
  "description": "Latest iPhone model with 256GB storage",
  "specifications": {
    "weight": 187,
    "dimensions": {
      "length": 14.67,
      "width": 7.09,
      "height": 0.83,
      "unit": "cm"
    },
    "color": "Black",
    "material": "Titanium"
  },
  "pricing": {
    "sellingPrice": 999.99,
    "msrp": 1199.99,
    "currency": "USD"
  },
  "inventory": {
    "currentStock": 0,
    "reorderLevel": 10,
    "maxStock": 100
  },
  "images": ["https://example.com/image1.jpg"],
  "tags": ["smartphone", "apple", "premium"]
}

// Response (201 Created)
{
  "success": true,
  "message": "Product created successfully",
  "data": {
    "_id": "64f1234567890abcdef12345",
    "name": "iPhone 15 Pro",
    "sku": "IP15P-256-BLK",
    // ... full product object
    "createdAt": "2025-10-15T10:00:00.000Z",
    "updatedAt": "2025-10-15T10:00:00.000Z"
  }
}
```

#### GET /api/products/:id/suppliers
```javascript
// Response (200 OK)
{
  "success": true,
  "data": {
    "product": {
      "_id": "64f1234567890abcdef12345",
      "name": "iPhone 15 Pro",
      "sku": "IP15P-256-BLK"
    },
    "suppliers": [
      {
        "_id": "64f1234567890abcdef22222",
        "supplierId": "64f1234567890abcdef33333",
        "supplierName": "Apple Authorized Distributor",
        "cost": 750.00,
        "currency": "USD",
        "isPrimary": true,
        "leadTime": 5,
        "minimumOrderQuantity": 10,
        "supplierProductCode": "AAD-IP15P-256-BLK",
        "isActive": true,
        "lastOrderDate": "2025-10-10T00:00:00.000Z"
      },
      {
        "_id": "64f1234567890abcdef22223",
        "supplierId": "64f1234567890abcdef33334",
        "supplierName": "Tech Wholesale Inc",
        "cost": 720.00,
        "currency": "USD",
        "isPrimary": false,
        "leadTime": 10,
        "minimumOrderQuantity": 25,
        "supplierProductCode": "TWI-IPHONE15PRO-256",
        "isActive": true
      }
    ]
  }
}
```

#### GET /api/products/:id/suppliers/best
```javascript
// Query Parameters
?quantity=30&urgency=normal&budget=25000

// Response (200 OK)
{
  "success": true,
  "data": {
    "recommendedSupplier": {
      "_id": "64f1234567890abcdef22222",
      "supplierId": "64f1234567890abcdef33333",
      "supplierName": "Apple Authorized Distributor",
      "cost": 750.00,
      "totalCost": 22500.00,
      "leadTime": 5,
      "score": 85,
      "reasons": [
        "Primary supplier",
        "Meets quantity requirement",
        "Acceptable lead time",
        "Within budget"
      ]
    },
    "alternatives": [
      {
        "_id": "64f1234567890abcdef22223",
        "supplierId": "64f1234567890abcdef33334",
        "supplierName": "Tech Wholesale Inc",
        "cost": 720.00,
        "totalCost": 21600.00,
        "leadTime": 10,
        "score": 78,
        "reasons": [
          "Lower cost option",
          "Meets minimum order quantity",
          "Longer lead time"
        ]
      }
    ]
  }
}
```

### Order Management Endpoints

#### POST /api/orders
```javascript
// Request
{
  "customerId": "64f1234567890abcdef44444",
  "items": [
    {
      "productId": "64f1234567890abcdef12345",
      "quantity": 2,
      "unitPrice": 999.99,
      "discount": 50.00
    },
    {
      "productId": "64f1234567890abcdef12346",
      "quantity": 1,
      "unitPrice": 299.99
    }
  ],
  "shippingInfo": {
    "address": {
      "street": "123 Main St",
      "city": "New York",
      "state": "NY",
      "zipCode": "10001",
      "country": "USA"
    },
    "method": "Standard"
  },
  "notes": "Rush order - needed by Friday"
}

// Response (201 Created)
{
  "success": true,
  "message": "Order created successfully",
  "data": {
    "_id": "64f1234567890abcdef55555",
    "orderNumber": "ORD-2025-000001",
    "customerId": "64f1234567890abcdef44444",
    "items": [
      {
        "productId": "64f1234567890abcdef12345",
        "name": "iPhone 15 Pro",
        "sku": "IP15P-256-BLK",
        "quantity": 2,
        "unitPrice": 999.99,
        "totalPrice": 1999.98,
        "discount": 50.00
      }
    ],
    "pricing": {
      "subtotal": 2249.97,
      "taxRate": 0.08,
      "taxAmount": 179.998,
      "discountAmount": 50.00,
      "shippingCost": 15.00,
      "totalAmount": 2394.968
    },
    "status": "Pending",
    "paymentStatus": "Unpaid",
    "dates": {
      "orderDate": "2025-10-15T10:00:00.000Z"
    },
    "createdAt": "2025-10-15T10:00:00.000Z"
  }
}
```

#### PATCH /api/orders/:id/status
```javascript
// Request
{
  "status": "Processing",
  "notes": "Order confirmed and being prepared"
}

// Response (200 OK)
{
  "success": true,
  "message": "Order status updated successfully",
  "data": {
    "_id": "64f1234567890abcdef55555",
    "status": "Processing",
    "updatedAt": "2025-10-15T11:00:00.000Z"
  }
}
```

### Purchase Management Endpoints

#### POST /api/purchases
```javascript
// Request
{
  "supplierId": "64f1234567890abcdef33333",
  "items": [
    {
      "productId": "64f1234567890abcdef12345",
      "productSupplierId": "64f1234567890abcdef22222",
      "quantity": 50,
      "unitCost": 750.00
    }
  ],
  "expectedDelivery": "2025-10-25T00:00:00.000Z",
  "notes": "Monthly inventory replenishment"
}

// Response (201 Created)
{
  "success": true,
  "message": "Purchase order created successfully",
  "data": {
    "_id": "64f1234567890abcdef66666",
    "purchaseNumber": "PUR-2025-000001",
    "supplierId": "64f1234567890abcdef33333",
    "items": [
      {
        "productId": "64f1234567890abcdef12345",
        "productSupplierId": "64f1234567890abcdef22222",
        "name": "iPhone 15 Pro",
        "sku": "IP15P-256-BLK",
        "supplierProductCode": "AAD-IP15P-256-BLK",
        "quantity": 50,
        "unitCost": 750.00,
        "totalCost": 37500.00,
        "receivedQuantity": 0
      }
    ],
    "pricing": {
      "subtotal": 37500.00,
      "taxRate": 0.08,
      "taxAmount": 3000.00,
      "shippingCost": 150.00,
      "totalCost": 40650.00
    },
    "status": "Draft",
    "paymentStatus": "Unpaid",
    "dates": {
      "purchaseDate": "2025-10-15T10:00:00.000Z",
      "expectedDelivery": "2025-10-25T00:00:00.000Z"
    },
    "createdAt": "2025-10-15T10:00:00.000Z"
  }
}
```

#### POST /api/purchases/:id/receive
```javascript
// Request
{
  "items": [
    {
      "productId": "64f1234567890abcdef12345",
      "receivedQuantity": 48,
      "notes": "2 units damaged in shipping"
    }
  ],
  "receivalDate": "2025-10-20T14:30:00.000Z",
  "notes": "Partial delivery - remaining 2 units to follow"
}

// Response (200 OK)
{
  "success": true,
  "message": "Purchase receival recorded successfully",
  "data": {
    "purchaseId": "64f1234567890abcdef66666",
    "status": "PartiallyReceived",
    "inventoryUpdates": [
      {
        "productId": "64f1234567890abcdef12345",
        "previousStock": 25,
        "receivedQuantity": 48,
        "newStock": 73
      }
    ]
  }
}
```

### Inventory Management Endpoints

#### GET /api/inventory
```javascript
// Query Parameters
?productId=64f1234567890abcdef12345&type=Purchase&startDate=2025-10-01&endDate=2025-10-31&page=1&limit=20

// Response (200 OK)
{
  "success": true,
  "data": {
    "transactions": [
      {
        "_id": "64f1234567890abcdef77777",
        "productId": "64f1234567890abcdef12345",
        "productName": "iPhone 15 Pro",
        "productSku": "IP15P-256-BLK",
        "transactionType": "Purchase",
        "referenceType": "Purchase",
        "referenceId": "64f1234567890abcdef66666",
        "referenceNumber": "PUR-2025-000001",
        "quantity": 48,
        "unitCost": 750.00,
        "totalValue": 36000.00,
        "previousStock": 25,
        "newStock": 73,
        "performedBy": "64f1234567890abcdef88888",
        "performedByName": "John Doe",
        "transactionDate": "2025-10-20T14:30:00.000Z"
      }
    ],
    "pagination": {
      "currentPage": 1,
      "totalPages": 1,
      "totalItems": 5,
      "itemsPerPage": 20
    }
  }
}
```

#### POST /api/inventory/adjustment
```javascript
// Request
{
  "productId": "64f1234567890abcdef12345",
  "adjustmentType": "Damaged",
  "quantity": -5,
  "unitCost": 750.00,
  "notes": "Water damage from warehouse leak",
  "location": {
    "warehouse": "Main",
    "section": "A",
    "shelf": "A1-05"
  }
}

// Response (201 Created)
{
  "success": true,
  "message": "Inventory adjustment recorded successfully",
  "data": {
    "_id": "64f1234567890abcdef77778",
    "productId": "64f1234567890abcdef12345",
    "transactionType": "Adjustment",
    "quantity": -5,
    "previousStock": 73,
    "newStock": 68,
    "transactionDate": "2025-10-15T15:00:00.000Z"
  }
}
```

### Dashboard & Analytics Endpoints

#### GET /api/dashboard/stats
```javascript
// Response (200 OK)
{
  "success": true,
  "data": {
    "overview": {
      "totalProducts": 247,
      "totalOrders": 1543,
      "totalCustomers": 892,
      "totalSuppliers": 45
    },
    "sales": {
      "today": 15642.50,
      "thisWeek": 87234.75,
      "thisMonth": 342567.25,
      "thisYear": 2847539.80
    },
    "inventory": {
      "totalValue": 1256789.50,
      "lowStockItems": 23,
      "outOfStockItems": 7,
      "reorderAlerts": 15
    },
    "orders": {
      "pending": 45,
      "processing": 23,
      "shipped": 67,
      "completed": 892,
      "cancelled": 12
    },
    "topSellingProducts": [
      {
        "productId": "64f1234567890abcdef12345",
        "name": "iPhone 15 Pro",
        "sku": "IP15P-256-BLK",
        "unitsSold": 156,
        "revenue": 155844.00
      }
    ],
    "recentActivity": [
      {
        "type": "order",
        "description": "New order ORD-2025-000156 created",
        "timestamp": "2025-10-15T14:30:00.000Z"
      }
    ]
  }
}
```

#### GET /api/reports/low-stock
```javascript
// Query Parameters
?threshold=10&categoryId=64f1234567890abcdef11111

// Response (200 OK)
{
  "success": true,
  "data": {
    "products": [
      {
        "_id": "64f1234567890abcdef12345",
        "name": "iPhone 15 Pro",
        "sku": "IP15P-256-BLK",
        "categoryName": "Electronics",
        "currentStock": 8,
        "reorderLevel": 10,
        "maxStock": 100,
        "suggestedReorderQuantity": 92,
        "primarySupplier": {
          "supplierId": "64f1234567890abcdef33333",
          "name": "Apple Authorized Distributor",
          "cost": 750.00,
          "leadTime": 5
        },
        "lastSaleDate": "2025-10-14T10:00:00.000Z",
        "averageDailySales": 2.5
      }
    ],
    "summary": {
      "totalLowStockItems": 23,
      "totalValue": 45678.90,
      "estimatedReorderCost": 234567.80
    }
  }
}
```

---

## Authentication & Authorization

### JWT Token Structure
```javascript
// Access Token Payload
{
  "sub": "64f1234567890abcdef12345", // User ID
  "username": "john_doe",
  "email": "john@example.com",
  "role": "Employee",
  "iat": 1697369400, // Issued at
  "exp": 1697373000, // Expires at (1 hour)
  "type": "access"
}

// Refresh Token Payload
{
  "sub": "64f1234567890abcdef12345",
  "iat": 1697369400,
  "exp": 1699961400, // Expires at (30 days)
  "type": "refresh",
  "jti": "unique-token-id" // For token revocation
}
```

### Role-Based Permissions
```javascript
const permissions = {
  Admin: {
    users: ['create', 'read', 'update', 'delete'],
    products: ['create', 'read', 'update', 'delete'],
    categories: ['create', 'read', 'update', 'delete'],
    suppliers: ['create', 'read', 'update', 'delete'],
    customers: ['create', 'read', 'update', 'delete'],
    orders: ['create', 'read', 'update', 'delete'],
    purchases: ['create', 'read', 'update', 'delete'],
    inventory: ['create', 'read', 'update', 'delete'],
    transactions: ['create', 'read', 'update', 'delete'],
    invoices: ['create', 'read', 'update', 'delete'],
    reports: ['read']
  },
  Manager: {
    users: ['read'],
    products: ['create', 'read', 'update'],
    categories: ['read'],
    suppliers: ['create', 'read', 'update'],
    customers: ['create', 'read', 'update'],
    orders: ['create', 'read', 'update'],
    purchases: ['create', 'read', 'update'],
    inventory: ['create', 'read', 'update'],
    transactions: ['create', 'read'],
    invoices: ['create', 'read', 'update'],
    reports: ['read']
  },
  Employee: {
    products: ['read'],
    categories: ['read'],
    suppliers: ['read'],
    customers: ['read', 'update'],
    orders: ['create', 'read', 'update'],
    purchases: ['read'],
    inventory: ['read'],
    transactions: ['read'],
    invoices: ['read'],
    reports: ['read']
  },
  Viewer: {
    products: ['read'],
    categories: ['read'],
    suppliers: ['read'],
    customers: ['read'],
    orders: ['read'],
    purchases: ['read'],
    inventory: ['read'],
    transactions: ['read'],
    invoices: ['read'],
    reports: ['read']
  }
};
```

### Middleware Implementation Guidelines
```javascript
// Authentication Middleware
const authenticateToken = (req, res, next) => {
  const authHeader = req.headers['authorization'];
  const token = authHeader && authHeader.split(' ')[1];
  
  if (!token) {
    return res.status(401).json({
      success: false,
      error: {
        code: 'MISSING_TOKEN',
        message: 'Access token is required'
      }
    });
  }
  
  jwt.verify(token, process.env.JWT_ACCESS_SECRET, (err, user) => {
    if (err) {
      return res.status(403).json({
        success: false,
        error: {
          code: 'INVALID_TOKEN',
          message: 'Invalid or expired token'
        }
      });
    }
    req.user = user;
    next();
  });
};

// Authorization Middleware
const authorize = (resource, action) => {
  return (req, res, next) => {
    const userRole = req.user.role;
    const userPermissions = permissions[userRole];
    
    if (!userPermissions || !userPermissions[resource] || !userPermissions[resource].includes(action)) {
      return res.status(403).json({
        success: false,
        error: {
          code: 'INSUFFICIENT_PERMISSIONS',
          message: 'You do not have permission to perform this action'
        }
      });
    }
    next();
  };
};
```

---

## Business Logic Rules

### Inventory Management Rules
1. **Stock Updates**: Automatically update product stock when orders are fulfilled or purchases are received
2. **Low Stock Alerts**: Generate alerts when stock falls below reorder level
3. **Negative Stock Prevention**: Prevent orders that would result in negative stock (configurable)
4. **Batch Tracking**: Track inventory movements with full audit trail

### Order Processing Workflow
1. **Order Creation**: Validate customer, products, and stock availability
2. **Stock Reservation**: Reserve inventory when order is confirmed
3. **Payment Processing**: Handle payment status updates
4. **Fulfillment**: Update inventory when items are shipped
5. **Completion**: Finalize order and generate invoice

### Purchase Order Workflow
1. **PO Creation**: Create purchase order with selected supplier
2. **Approval Process**: Route for approval based on amount thresholds
3. **Sending**: Send PO to supplier
4. **Receiving**: Record received quantities and update inventory
5. **Invoice Matching**: Match supplier invoices with PO

### Supplier Selection Logic
```javascript
const selectBestSupplier = (productId, quantity, criteria = {}) => {
  const { urgency = 'normal', maxBudget, preferredSupplier } = criteria;
  
  // Score suppliers based on multiple factors
  const scoringWeights = {
    cost: 0.4,
    leadTime: 0.3,
    reliability: 0.2,
    minimumOrder: 0.1
  };
  
  // Apply urgency adjustments
  if (urgency === 'high') {
    scoringWeights.leadTime = 0.6;
    scoringWeights.cost = 0.2;
  }
  
  // Calculate scores and return recommendation
};
```

### Pricing Rules
1. **Dynamic Pricing**: Support for tiered pricing based on quantity
2. **Currency Handling**: Multi-currency support with conversion rates
3. **Tax Calculation**: Automatic tax calculation based on location and product type
4. **Discount Management**: Support for customer-specific and promotional discounts

---

## Error Handling

### Standard Error Response Format
```javascript
{
  "success": false,
  "error": {
    "code": "ERROR_CODE",
    "message": "Human-readable error message",
    "details": [], // Optional array of detailed errors
    "timestamp": "2025-10-15T10:00:00.000Z",
    "path": "/api/products",
    "method": "POST"
  }
}
```

### Error Codes
```javascript
const ERROR_CODES = {
  // Authentication & Authorization
  INVALID_CREDENTIALS: 'Invalid email or password',
  MISSING_TOKEN: 'Access token is required',
  INVALID_TOKEN: 'Invalid or expired token',
  INSUFFICIENT_PERMISSIONS: 'Insufficient permissions for this action',
  ACCOUNT_DISABLED: 'Account has been disabled',
  
  // Validation
  VALIDATION_ERROR: 'Validation failed',
  REQUIRED_FIELD_MISSING: 'Required field is missing',
  INVALID_FORMAT: 'Invalid data format',
  DUPLICATE_VALUE: 'Value already exists',
  
  // Business Logic
  INSUFFICIENT_STOCK: 'Insufficient stock available',
  INVALID_QUANTITY: 'Invalid quantity specified',
  PRODUCT_NOT_ACTIVE: 'Product is not active',
  ORDER_NOT_EDITABLE: 'Order cannot be modified in current status',
  
  // System
  DATABASE_ERROR: 'Database operation failed',
  EXTERNAL_SERVICE_ERROR: 'External service unavailable',
  RATE_LIMIT_EXCEEDED: 'Rate limit exceeded'
};
```

### HTTP Status Codes Usage
- **200 OK**: Successful GET, PUT, PATCH requests
- **201 Created**: Successful POST requests
- **204 No Content**: Successful DELETE requests
- **400 Bad Request**: Validation errors, malformed requests
- **401 Unauthorized**: Authentication required
- **403 Forbidden**: Authorization failed
- **404 Not Found**: Resource not found
- **409 Conflict**: Duplicate resources, business rule violations
- **422 Unprocessable Entity**: Semantic validation errors
- **429 Too Many Requests**: Rate limiting
- **500 Internal Server Error**: System errors

---

## Validation Rules

### Product Validation
```javascript
const productValidation = {
  name: {
    required: true,
    type: 'string',
    minLength: 2,
    maxLength: 200,
    trim: true
  },
  sku: {
    required: true,
    type: 'string',
    pattern: /^[A-Z0-9-]+$/,
    unique: true,
    transform: 'uppercase'
  },
  categoryId: {
    required: true,
    type: 'objectId',
    exists: 'categories'
  },
  pricing: {
    sellingPrice: {
      required: true,
      type: 'number',
      min: 0
    }
  },
  inventory: {
    currentStock: {
      type: 'number',
      min: 0,
      default: 0
    },
    reorderLevel: {
      type: 'number',
      min: 0,
      default: 10
    }
  }
};
```

### Order Validation
```javascript
const orderValidation = {
  customerId: {
    required: true,
    type: 'objectId',
    exists: 'customers'
  },
  items: {
    required: true,
    type: 'array',
    minItems: 1,
    items: {
      productId: {
        required: true,
        type: 'objectId',
        exists: 'products'
      },
      quantity: {
        required: true,
        type: 'number',
        min: 1,
        integer: true
      },
      unitPrice: {
        required: true,
        type: 'number',
        min: 0
      }
    }
  }
};
```

---

## System Architecture

### Project Structure
```
inventory-management-system/
├── src/
│   ├── controllers/
│   │   ├── auth.controller.js
│   │   ├── products.controller.js
│   │   ├── orders.controller.js
│   │   └── ...
│   ├── middleware/
│   │   ├── auth.middleware.js
│   │   ├── validation.middleware.js
│   │   └── error.middleware.js
│   ├── models/
│   │   ├── User.model.js
│   │   ├── Product.model.js
│   │   └── ...
│   ├── routes/
│   │   ├── auth.routes.js
│   │   ├── products.routes.js
│   │   └── ...
│   ├── services/
│   │   ├── auth.service.js
│   │   ├── inventory.service.js
│   │   └── ...
│   ├── utils/
│   │   ├── database.js
│   │   ├── logger.js
│   │   └── helpers.js
│   ├── config/
│   │   ├── database.config.js
│   │   └── auth.config.js
│   └── app.js
├── tests/
├── docs/
├── package.json
└── .env
```

### Environment Variables
```bash
# Database
MONGODB_URI=mongodb://localhost:27017/inventory_management
MONGODB_TEST_URI=mongodb://localhost:27017/inventory_management_test

# JWT
JWT_ACCESS_SECRET=your_access_token_secret
JWT_REFRESH_SECRET=your_refresh_token_secret
JWT_ACCESS_EXPIRY=1h
JWT_REFRESH_EXPIRY=30d

# Server
PORT=3000
NODE_ENV=development

# Security
BCRYPT_ROUNDS=12
RATE_LIMIT_WINDOW=15
RATE_LIMIT_MAX_REQUESTS=100

# Email (for notifications)
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_USER=your_email@gmail.com
SMTP_PASS=your_app_password

# File Upload
UPLOAD_PATH=./uploads
MAX_FILE_SIZE=5MB
ALLOWED_EXTENSIONS=jpg,jpeg,png,pdf

# External APIs
CURRENCY_API_KEY=your_currency_api_key
SHIPPING_API_KEY=your_shipping_api_key
```

### Security Best Practices
1. **Password Hashing**: Use bcrypt with minimum 12 rounds
2. **Rate Limiting**: Implement API rate limiting
3. **Input Validation**: Validate and sanitize all inputs
4. **SQL Injection Prevention**: Use parameterized queries/ODM
5. **CORS Configuration**: Properly configure CORS
6. **Security Headers**: Use helmet.js for security headers
7. **Environment Variables**: Store sensitive data in environment variables
8. **Audit Logging**: Log all critical operations

This comprehensive specification gives you everything you need to build a professional inventory management system. Each section provides detailed implementation guidance while maintaining flexibility for your specific requirements.