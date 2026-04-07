# Thrift & Handmade Store E-Commerce System

Welcome to the Thrift & Handmade Store project! This repository contains the backend database schema and infrastructure for an e-commerce platform dedicated to selling thrifted clothing and exclusively handmade items.

## 📌Project Overview

This project provides a robust data model to support a full-featured online store. It handles everything from user registration and product catalogs to shopping carts, wishlists, and order processing. The store uniquely categorizes inventory into two distinct types: **Thrift** (second-hand goods) and **Handmade** (custom-crafted items).

## 🚀 Key Features

Based on the core database architecture, the system supports the following features:

- **Customer Management System**: Track customer profiles, contact numbers, secure passwords, profile pictures, and delivery addresses.
- **Product Catalog**: Manage diverse products with high-quality images, descriptions, pricing, and type classification (Thrift or Handmade).
- **Inventory Management (Stocks)**: Real-time tracking of product availability and quantities in the warehouse.
- **Shopping Cart System**: Allows users to add multiple products to their cart before proceeding to checkout.
- **Order Processing**: Comprehensive tracking of orders, including overall order totals, payment status, shipping status, placed dates, and expected delivery dates.
- **Order Items Support**: Customers can place a single unified order that contains multiple different products and quantities.
- **Wishlist Functionality**: Customers can save their favorite items and track the current stock availability.

## 🗄️ Database Schema Representation

The application relies on a strictly typed relational database model. Here is an overview of the core entities and their relationships:

### Core Tables

1. **`customer`**: Stores user authentication and profile details.
   - *Fields*: `customer_id` (PK), `customer_name`, `customer_email`, `customer_password`, `customer_profile_pic`, `customer_contact_number`, `customer_delivery_address`, etc.
2. **`product`**: Holds information about each item for sale.
   - *Fields*: `product_id` (PK), `product_image`, `product_description`, `product_price`, `product_type_thrift` (Boolean flag: True for Thrift, False for Handmade).
3. **`stocks`**: Manages the available warehouse inventory for each product.
   - *Fields*: `product_id` (FK), `quantity_available`.
4. **`cart`**: A persistent shopping cart linking customers to multiple products.
   - *Fields*: `customer_id` (FK), `product_id` (FK) - Composite Key.
5. **`orders`**: Represents a completed checkout instance.
   - *Fields*: `order_id` (PK), `customer_id` (FK), `is_payed`, `is_shipped`, `total_price`, `expected_delivery`, etc.
6. **`order_items`**: Line items for each order, breaking down individual products and quantities purchased.
   - *Fields*: `order_id` (FK), `product_id` (FK), `product_quantity`.
7. **`wishlist`**: Items favorited by users.
   - *Fields*: `customer_id` (FK), `product_id` (FK), `product_availability` (FK mapped to stocks).

## 📊 Entity Relationship Summary

- **Customer to Cart/Orders/Wishlist**: One-to-Many relationships. A customer can have multiple items in their cart, multiple past orders, and multiple wishlist items.
- **Order to Order Items**: One-to-Many. An order can contain multiple specific items.
- **Product to Stocks**: One-to-One. Each product has a linked stock record.
- **Product to Order Items/Cart/Wishlist**: One-to-Many. A specific product can be in multiple carts, wishlists, and past order histories.

