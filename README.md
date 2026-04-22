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