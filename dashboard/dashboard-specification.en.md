# Dashboard Specification

---

## 1. Overview

The Dashboard is an operational management system for online pickup orders only.  
It is NOT a replacement for the POS system (Foodics).

### Objectives
- Monitor online pickup orders
- Control order flow and prevent congestion
- Manage pickup time slots
- Monitor Foodics integration
- Provide operational visibility

---

## 2. Roles & Permissions

### Super Admin

**Description:**  
Full system visibility and control.

**Scope:**
- All branches
- All orders
- All settings
- Integrations
- User management

---

### Admin (Branch Manager / Operations Supervisor)

**Description:**  
Responsible for a single branch.

**Scope:**
- Assigned branch only
- Active orders
- Load and preparation control

---

## 3. Navigation Structure

Sidebar includes:

1. Dashboard
2. Orders
3. Pickup Slots
4. Branch Settings
5. Menu (Read Only)
6. Foodics Integration
7. Reports
8. Users & Roles (Super Admin Only)

---

## 4. Main Dashboard

### Super Admin View
- Orders today
- Active orders
- Average preparation time
- Foodics connection status
- Open / Closed branches
- Error alerts

### Admin View
- Branch active orders
- Orders within next 30 minutes
- Branch status (Active / Paused)
- Load indicator (Low / Medium / High)

---

## 5. Orders Management

### Displayed Data
- Order number
- Branch
- Pickup time
- Status
- Payment method
- Pickup code / QR

### Admin Permissions
- View branch orders
- View details
- Change status (policy-based)
- Cancel before preparation
- Add internal note

Not Allowed:
- Edit prices
- Modify order items

### Super Admin Permissions
- View all orders
- Advanced filtering
- Export CSV

---

## 6. Pickup Slots & Load Control

### Super Admin
- Define slot duration
- Define default capacity
- Global emergency settings

### Admin
- Temporarily increase preparation time
- Temporarily reduce capacity
- Pause order acceptance

Temporary changes must include:
- Start time
- End time
- Optional reason

---

## 7. Branch Settings

### Super Admin
- Create / Edit branches
- Working hours
- Base preparation time
- Activate / Deactivate branch

### Admin
- View only
- Adjust temporary preparation time

---

## 8. Menu (Read Only)

Menu is synchronized from Foodics.

Includes:
- Categories
- Products
- Prices
- Add-ons
- Availability

Rules:
- No create
- No edit
- No delete

Buttons:
- Sync Menu
- View last sync
- View sync errors

---

## 9. Foodics Integration

(Super Admin Only)

Displays:
- Connection status
- Last webhook
- Failed orders
- Retry attempts
- Logs

---

## 10. Reports

### Super Admin
- Daily order count
- Peak hours
- Average preparation time
- Slot usage

### Admin
- Branch-only reports
- View only

---

## 11. System Rules

1. Dashboard does not modify menu or prices.
2. Foodics is the source of truth.
3. Operational changes must be temporary and traceable.
4. Sensitive actions require confirmation.
5. Permissions control visibility and actions.

---

## 12. Error Handling

- Foodics failure → Alert
- Full slots → Block orders
- Webhook failure → Retry + Log
- Preparation delay → Warning

---

## Conclusion

- Single unified dashboard
- Clear role separation
- Operational control only
- Foodics manages menu
- Built for real-world operations
