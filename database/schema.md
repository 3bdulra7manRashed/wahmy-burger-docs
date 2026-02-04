# 🗄 Database Schema — Wahmy Platform (Final)

This document defines the **complete and final database schema** for the Wahmy pickup-only ordering platform.  
Rendered automatically as tables on GitHub.

---

## 👤 Users

Stores all system users (customers, admins, super admins).

| Column        | Type                       | Notes |
|--------------|----------------------------|------|
| id           | BIGINT (PK)                | |
| phone        | VARCHAR(20) NULL           | Customer login (OTP) |
| country_code | VARCHAR(5) NULL            | Example: +20, +966 |
| email        | VARCHAR(255) NULL          | Admin login |
| password     | VARCHAR(255) NULL          | Hashed (admins only) |
| name         | VARCHAR(255) NULL          | |
| gender       | ENUM('male','female') NULL | Optional |
| birth_date   | DATE NULL                  | Optional |
| country      | VARCHAR(100) NULL          | Optional |
| created_at   | TIMESTAMP                  | |
| updated_at   | TIMESTAMP                  | |

---

## 🔐 Roles

Defines system roles.

| Column | Type | Notes |
|------|------|------|
| id   | BIGINT (PK) | |
| name | VARCHAR(50) | super_admin / admin / customer |

---

## 🔗 User Roles

Maps users to roles and branches.

| Column     | Type        | Notes |
|-----------|-------------|------|
| id        | BIGINT (PK) | |
| user_id   | BIGINT (FK) | references users.id |
| role_id   | BIGINT (FK) | references roles.id |
| branch_id | BIGINT NULL | Admin only |

---

## 🔐 OTP Codes

Stores OTP verification data for customers.

| Column        | Type             | Notes |
|--------------|------------------|------|
| id           | BIGINT (PK)      | |
| phone        | VARCHAR(20)      | |
| country_code | VARCHAR(5)       | |
| otp_code     | VARCHAR(10)      | |
| expires_at   | TIMESTAMP        | |
| verified_at  | TIMESTAMP NULL   | |
| created_at   | TIMESTAMP        | |

---

## 🏪 Branches

Restaurant branches.

| Column             | Type            | Notes |
|-------------------|-----------------|------|
| id                | BIGINT (PK)     | |
| name              | VARCHAR(255)    | |
| city              | VARCHAR(100)    | |
| latitude          | DECIMAL(10,8)   | |
| longitude         | DECIMAL(11,8)   | |
| opens_at          | TIME            | |
| closes_at         | TIME            | |
| is_active         | BOOLEAN         | |
| base_prep_minutes | INT             | |
| created_at        | TIMESTAMP       | |
| updated_at        | TIMESTAMP       | |

---

## 📂 Categories

Menu categories.

| Column      | Type            | Notes |
|------------|-----------------|------|
| id         | BIGINT (PK)     | |
| name       | VARCHAR(255)    | |
| position   | INT             | Sorting order |
| is_active  | BOOLEAN         | |
| created_at | TIMESTAMP       | |
| updated_at | TIMESTAMP       | |

---

## 🍔 Products

Menu products.

| Column       | Type            | Notes |
|-------------|-----------------|------|
| id          | BIGINT (PK)     | |
| category_id | BIGINT (FK)     | |
| name        | VARCHAR(255)    | |
| description | TEXT            | |
| image       | VARCHAR(255)    | |
| is_active   | BOOLEAN         | |
| created_at  | TIMESTAMP       | |
| updated_at  | TIMESTAMP       | |

---

## 📏 Product Variants

Product sizes / variations.

| Column      | Type            | Notes |
|------------|-----------------|------|
| id         | BIGINT (PK)     | |
| product_id | BIGINT (FK)     | |
| name       | VARCHAR(100)    | Small / Medium / Large |
| price      | DECIMAL(10,2)   | |
| created_at | TIMESTAMP       | |
| updated_at | TIMESTAMP       | |

---

## ➕ Extras

Optional add-ons.

| Column      | Type            | Notes |
|------------|-----------------|------|
| id         | BIGINT (PK)     | |
| name       | VARCHAR(255)    | |
| price      | DECIMAL(10,2)   | |
| created_at | TIMESTAMP       | |
| updated_at | TIMESTAMP       | |

---

## 🔗 Product Extras

Pivot table between products and extras.

| Column     | Type        | Notes |
|-----------|-------------|------|
| id        | BIGINT (PK) | |
| product_id| BIGINT (FK) | |
| extra_id  | BIGINT (FK) | |

---

## 🧾 Orders

Customer orders (pickup only).

| Column          | Type        | Notes |
|----------------|-------------|------|
| id             | BIGINT (PK) | |
| order_number   | VARCHAR(50) | Unique |
| branch_id      | BIGINT (FK) | |
| user_id        | BIGINT NULL | |
| status         | ENUM        | pending / confirmed / preparing / ready / picked_up / canceled |
| pickup_type    | ENUM        | asap / scheduled |
| pickup_at      | TIMESTAMP   | |
| subtotal       | DECIMAL(10,2)| |
| tax            | DECIMAL(10,2)| |
| total          | DECIMAL(10,2)| |
| payment_method | ENUM        | online / cod |
| payment_status | ENUM        | pending / paid / failed |
| pickup_code    | VARCHAR(20) | Unique |
| created_at     | TIMESTAMP   | |
| updated_at     | TIMESTAMP   | |

---

## 🧾 Order Items

Products inside orders.

| Column              | Type        | Notes |
|--------------------|-------------|------|
| id                 | BIGINT (PK) | |
| order_id           | BIGINT (FK) | |
| product_variant_id | BIGINT (FK) | |
| quantity           | INT         | |
| unit_price         | DECIMAL(10,2)| Stored at order time |
| created_at         | TIMESTAMP   | |
| updated_at         | TIMESTAMP   | |

---

## ➕ Order Item Extras

Extras per order item.

| Column        | Type        | Notes |
|--------------|-------------|------|
| id           | BIGINT (PK) | |
| order_item_id| BIGINT (FK) | |
| extra_id     | BIGINT (FK) | |
| price        | DECIMAL(10,2)| Stored at order time |

---

## ⏱ Pickup Slots

Controls pickup scheduling and capacity.

| Column          | Type        | Notes |
|----------------|-------------|------|
| id             | BIGINT (PK) | |
| branch_id      | BIGINT (FK) | |
| slot_time      | TIMESTAMP   | |
| capacity       | INT         | |
| reserved_count | INT         | Prevent overbooking |
| created_at     | TIMESTAMP   | |
| updated_at     | TIMESTAMP   | |

---

## 🔌 External Branches (Foodics)

| Column      | Type        | Notes |
|------------|-------------|------|
| id         | BIGINT (PK) | |
| branch_id  | BIGINT (FK) | |
| provider   | VARCHAR(50) | foodics |
| external_id| VARCHAR(255)| |
| created_at | TIMESTAMP   | |

---

## 🔌 External Products (Foodics)

| Column      | Type        | Notes |
|------------|-------------|------|
| id         | BIGINT (PK) | |
| product_id | BIGINT (FK) | |
| provider   | VARCHAR(50) | foodics |
| external_id| VARCHAR(255)| |
| created_at | TIMESTAMP   | |

---

## 🔌 External Orders (Foodics)

| Column      | Type        | Notes |
|------------|-------------|------|
| id         | BIGINT (PK) | |
| order_id   | BIGINT (FK) | |
| provider   | VARCHAR(50) | foodics |
| external_id| VARCHAR(255)| |
| status     | VARCHAR(50) | |
| created_at | TIMESTAMP   | |
| updated_at | TIMESTAMP   | |

---

## 📡 Integration Logs

| Column      | Type        | Notes |
|------------|-------------|------|
| id         | BIGINT (PK) | |
| provider   | VARCHAR(50) | |
| event      | VARCHAR(100)| |
| payload    | JSON        | |
| created_at | TIMESTAMP   | |

---

## ✅ Final Notes

- GitHub renders all tables automatically
- Foodics is the **source of truth** for menu data
- Customers authenticate via **Mobile + OTP**
- Admins authenticate via **Email + Password**
- Pickup slots must be reserved atomically
- Prices are stored at order time (no recalculation)

---