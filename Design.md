# Software Design Description (SDD)

## 1. Introduction

### 1.1 Purpose

The purpose of this Software Design Description (SDD) is to provide a detailed design for the e-commerce platform. This document outlines the system architecture, components, interfaces, and their interactions to meet the platform's functional and non-functional requirements.

### 1.2 Scope

The e-commerce platform facilitates online shopping, allowing users to browse products, add items to a cart, and make secure purchases. The system supports features such as product recommendations, inventory management, order tracking, and customer support. Components include a frontend application, backend services, and third-party integrations for payment and shipping.

### 1.3 Design Goals

The design aims to achieve the following objectives:

- Deliver a seamless and responsive user experience.
- Ensure scalability to handle high traffic and transaction volumes.
- Provide robust security and privacy measures.
- Enable personalized shopping experiences.
- Support easy integration with third-party systems.

### 1.4 Architectural Principles

The design adheres to these principles:

- Modular and scalable architecture.
- Cloud-native deployment for flexibility.
- API-first design for extensibility.
- Strong focus on data security and performance optimization.

---

## 2. System Architecture

### 2.1 Architecture Overview

The e-commerce platform employs a microservices-based architecture with the following layers:

#### Presentation Layer:

- **React** for web interfaces.
- **React Native** for mobile apps.

#### Application Layer:

- Microservices built with **Node.js** and **Express**.
- REST and GraphQL APIs.

#### Data Layer:

- Primary database: **MongoDB**.
- Media storage: **Cloudinary**.

---

## 3. System Capabilities

### 3.1 Core Features

- **User Management**: Account creation, login, and profile management.
- **Product Catalog**: Browsing, filtering, and search capabilities.
- **Shopping Cart**: Add/remove items, apply discounts, and checkout.
- **Order Management**: Track orders, manage returns, and view history.
- **Payment Integration**: Secure online payments via third-party providers.
- **Notifications**: Email and SMS alerts for order updates.

### 3.2 Advanced Features

- **Personalization**: Product recommendations based on user preferences.
- **Inventory Management**: Real-time stock updates and notifications.
- **Promotions**: Dynamic pricing, discounts, and coupon code management.
- **Customer Support**: Chatbots and ticketing system integration.

---

## 4. Module Design

### 4.1 Frontend Application

#### 4.1.1 User Interface (UI)

Key components:

- **Home Page**: Highlights featured products and deals.
- **Product Details**: Displays detailed product information and reviews.
- **Cart**: Provides a summary of selected items.
- **Checkout**: Facilitates payment and order confirmation.
- **User Dashboard**: Allows users to manage profiles and track orders.

#### 4.1.2 Controller Layer

Handles user interactions and communicates with backend services.

### 4.2 Backend Services

#### 4.2.1 API Gateway

- Centralized gateway for routing API requests.

#### 4.2.2 Authentication Service

- **OAuth2** for social logins.
- Token-based session management.

#### 4.2.3 Product Service

- Manages product catalog, inventory, and search.

#### 4.2.4 Order Service

- Handles order processing, payment integration, and tracking.

## 5. Swimlane diagram

### 5.1 for ordering a product
![alt text](<order swimlane.png>)

```
@startuml
|Customer|
start
:Select product(s);
:View cart;
:Proceed to checkout;

|Web/Mobile App|
:Display checkout page;
:Send order details to Order Service;

|Order Service|
:Validate order details;
:Check product availability;
if (Products available?) then (Yes)
  :Initiate payment process;
else (No)
  :Notify customer;
  stop
endif

|Payment Gateway|
:Process payment;
if (Payment successful?) then (Yes)
  :Send payment confirmation;
else (No)
  :Send payment failure notification;
  stop
endif

|Order Service|
:Update order status;
:Notify Seller for order fulfillment;

|Seller|
:Prepare product for shipping;
:Update shipping details;

|Notification Service|
:Send order confirmation to customer;
:Send shipping details to customer;

|Customer|
:Receive order confirmation;
stop
@enduml
```
### 5.2 for returning a product
![alt text](<return product swimlane.png>)


```
@startuml
|Customer|
start
:Request product return;
:Provide reason for return;
:Initiate return request;

|Web/Mobile App|
:Submit return request to Return Service;

|Return Service|
:Validate return eligibility;
if (Return eligible?) then (Yes)
  :Approve return;
  :Notify Customer;
  :Notify Seller;
else (No)
  :Reject return request;
  :Notify Customer;
  stop
endif

|Seller|
:Receive return request;
:Review and confirm return;

|Return Service|
:Schedule return pickup with Delivery Partner;
:Generate return shipping label;
:Update return status;

|Notification Service|
:Send pickup schedule to customer;
:Notify seller of product return;

|Delivery Partner|
:Pick up product from customer;
:Ship product to seller;

|Seller|
:Inspect returned product;
if (Product acceptable?) then (Yes)
  :Approve refund/exchange;
else (No)
  :Reject refund/exchange;
endif

|Return Service|
:Update refund/exchange status;
:Initiate refund/exchange process;

|Notification Service|
:Notify customer of refund/exchange status;

|Customer|
:Receive refund or exchange product;
stop
@enduml

```
## 6. Database Design

Entities include:

- **Users**: Customer data and preferences.
- **Products**: Details, categories, and stock levels.
- **Orders**: Purchase records and statuses.
- **Cart**: Temporary data for items in cart.

![Database Design](Database_Design.png)

### Plant UML Code

```
@startuml
entity User {
  *user_id : int
  --
  username : varchar
  email : varchar
  password_hash : varchar
  profile_picture : varchar
  join_date : datetime
  is_verified : boolean
}

entity Product {
  *product_id : int
  --
  name : varchar
  description : text
  price : decimal
  stock : int
  category_id : int
  brand : varchar
  created_date : datetime
  updated_date : datetime
}

entity Category {
  *category_id : int
  --
  name : varchar
  description : text
}

entity Order {
  *order_id : int
  --
  user_id : int
  total_amount : decimal
  status : varchar
  created_date : datetime
  shipping_address : text
}

entity OrderItem {
  *order_item_id : int
  --
  order_id : int
  product_id : int
  quantity : int
  price : decimal
}

entity Cart {
  *cart_id : int
  --
  user_id : int
}

entity CartItem {
  *cart_item_id : int
  --
  cart_id : int
  product_id : int
  quantity : int
}

entity Payment {
  *payment_id : int
  --
  order_id : int
  payment_method : varchar
  payment_status : varchar
  payment_date : datetime
}

entity Review {
  *review_id : int
  --
  user_id : int
  product_id : int
  rating : int
  comment : text
  created_date : datetime
}

entity Wishlist {
  *wishlist_id : int
  --
  user_id : int
}

entity WishlistItem {
  *wishlist_item_id : int
  --
  wishlist_id : int
  product_id : int
}

User ||--o{ Order : "places"
Order ||--o{ OrderItem : "contains"
Product ||--o{ OrderItem : "is in"
User ||--o{ Cart : "has"
Cart ||--o{ CartItem : "contains"
Product ||--o{ CartItem : "is in"
Category ||--o{ Product : "categorizes"
User ||--o{ Review : "writes"
Product ||--o{ Review : "receives"
User ||--o{ Wishlist : "maintains"
Wishlist ||--o{ WishlistItem : "contains"
Product ||--o{ WishlistItem : "is in"
Order ||--|| Payment : "has"
@enduml
```
---