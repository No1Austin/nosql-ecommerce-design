# NoSQL Database Design for an E-Commerce Application

## Part 1: Initial Schema Design Based on Requirements

### Objective
The goal of this design is to create a NoSQL database schema for an e-commerce application that supports:

- product browsing and search
- order storage with customer details, purchased items, and delivery status
- thousands of transactions per second

---

## 1. Key Entities

The main entities in the system are:

- **Users**
- **Products**
- **Orders**

---

## 2. Chosen NoSQL Model

A **document-based NoSQL database** (e.g., MongoDB) was selected.

### Reasons:
- Flexible schema for different product types
- Supports nested data (orders with items)
- Efficient for read-heavy workloads (product browsing)
- Scales horizontally for high traffic

---

## 3. Initial Schema Design

### Users Collection

```json
{
  "_id": "user_101",
  "name": "Alice Johnson",
  "email": "alice@example.com",
  "phone": "1234567890",
  "address": {
    "street": "12 Main Street",
    "city": "London",
    "zip": "E1 6AN",
    "country": "UK"
  },
  "createdAt": "2026-04-22T10:00:00Z"
}

Products Collection
{
  "_id": "prod_501",
  "name": "Wireless Mouse",
  "description": "Ergonomic wireless mouse with USB receiver",
  "category": "Electronics",
  "price": 25.99,
  "stock": 320,
  "tags": ["mouse", "wireless", "computer"],
  "rating": 4.5,
  "createdAt": "2026-04-22T10:10:00Z"
}


Orders Collection
{
  "_id": "order_9001",
  "userId": "user_101",
  "customerInfo": {
    "name": "Alice Johnson",
    "email": "alice@example.com",
    "address": {
      "street": "12 Main Street",
      "city": "London",
      "zip": "E1 6AN",
      "country": "UK"
    }
  },
  "items": [
    {
      "productId": "prod_501",
      "productName": "Wireless Mouse",
      "quantity": 2,
      "price": 25.99
    }
  ],
  "totalAmount": 51.98,
  "deliveryStatus": "Processing",
  "paymentStatus": "Paid",
  "createdAt": "2026-04-22T10:20:00Z"
}

Relationships
One user → many orders
One order → many products
Product data is embedded in orders to preserve historical accuracy

Indexes and Query Patterns
Users
Query: find by email
Index: email (unique)
Products
Queries:
browse by category
search by name/description
sort by price
Indexes:
category
price
text index on name, description
Orders
Queries:
find orders by user
filter by delivery status
recent orders
Indexes:
userId
deliveryStatus
createdAt


Scalability and Consistency
Products: optimized for fast reads and search
Orders: optimized for high write throughput
Strong consistency needed for payments and order status
Eventual consistency acceptable for product browsing
Part 2: Refactored Schema Design
New Requirements
large-scale analytics
high availability
partition tolerance
Refactor Strategy

The design was updated using:

Sharding
Replication
Denormalization

Sharding
Orders
Sharded by userId or hashed _id
Distributes high write load
Products
Sharded by category or hashed _id
Supports large-scale browsing

Replication
Data replicated across nodes
Enables failover if a node fails
Improves availability and fault tolerance
Denormalized Analytics Collections

Sales Summary
{
  "_id": "2026-04-22_prod_501",
  "date": "2026-04-22",
  "productId": "prod_501",
  "productName": "Wireless Mouse",
  "category": "Electronics",
  "unitsSold": 240,
  "revenue": 6237.60
}
Product Trends
{
  "_id": "trend_prod_501_2026_04",
  "productId": "prod_501",
  "month": "2026-04",
  "views": 15000,
  "purchases": 2400,
  "cartAdds": 4100
}
Category Summary
{
  "_id": "2026-04_Electronics",
  "month": "2026-04",
  "category": "Electronics",
  "totalSales": 210000,
  "unitsSold": 8200,
  "topProducts": [
    "Wireless Mouse",
    "Gaming Keyboard",
    "USB Hub"
  ]
}

Benefits
Improved scalability (sharding)
Improved availability (replication)
Faster analytics (denormalization)

Trade-offs
Increased complexity
More storage usage
Possible eventual consistency


Reflection Report
During the schema refactor, one of the main challenges was adapting the initial design to support both transactional and analytical workloads. The original schema was efficient for storing orders and products but was not optimized for large-scale analytics queries. This required introducing new structures without affecting existing functionality.

The new requirements significantly influenced the design. Sharding was used to distribute data and support high traffic, while replication improved availability and fault tolerance. Denormalized collections were introduced to store pre-aggregated data, allowing faster analytics queries without scanning large datasets.

The refactor improved scalability, availability, and query performance. However, it also introduced trade-offs such as increased complexity and additional storage requirements. Overall, this process demonstrated how NoSQL database design must evolve to meet changing system requirements while balancing performance and consistency.

