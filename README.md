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

These entities represent the core data needed for the e-commerce platform.

---

## 2. Chosen NoSQL Model

A **document-based NoSQL database** such as MongoDB is a suitable choice for this application.

### Why this model was chosen:
- It supports **flexible schemas**, which is useful for products with different attributes.
- It stores related data together in a single document.
- It handles **nested data** well, such as order items and customer information.
- It supports indexing and horizontal scaling for high performance.

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
---

# Part 2: Refactored Schema Design

## New Requirements

The system must now support:

- large-scale analytics queries (sales trends, product performance)
- high availability
- partition tolerance due to increased users

---

## Refactor Strategy

To meet these requirements, the design was updated using:

- **Sharding**
- **Replication**
- **Denormalization**

---

## 1. Sharding

Sharding is used to distribute data across multiple nodes to improve scalability.

### Orders Collection
- Sharded by `userId` or hashed `_id`
- This distributes high write traffic evenly across nodes

### Products Collection
- Can be sharded by `category` or hashed `_id`
- Supports large-scale product browsing

---

## 2. Replication

Replication is used to improve availability and fault tolerance.

- Data is copied across multiple nodes (replica set)
- If one node fails, another node takes over
- Ensures the system remains available

---

## 3. Denormalized Analytics Collections

New collections are added to support fast analytics queries.

### Sales Summary

```json
{
  "_id": "2026-04-22_prod_501",
  "date": "2026-04-22",
  "productId": "prod_501",
  "productName": "Wireless Mouse",
  "category": "Electronics",
  "unitsSold": 240,
  "revenue": 6237.60
}

{
  "_id": "trend_prod_501_2026_04",
  "productId": "prod_501",
  "month": "2026-04",
  "views": 15000,
  "purchases": 2400,
  "cartAdds": 4100
}

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