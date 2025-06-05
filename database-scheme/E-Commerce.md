## 🔑 Core Entities

Let’s start with the essential tables. You can later expand them with more features like promotions, reviews, inventory logs, etc.

## 🗃️ 1. Users Table

Stores information about customers (and optionally admins).

```sql
CREATE TABLE users (
  id UUID PRIMARY KEY,
  full_name VARCHAR(255) NOT NULL,
  email VARCHAR(255) UNIQUE NOT NULL,
  password_hash TEXT NOT NULL,
  phone VARCHAR(20),
  address TEXT,
  is_admin BOOLEAN DEFAULT FALSE,
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);
```

## 🛍️ 2. Products Table

Stores products being sold.

```sql
CREATE TABLE products (
  id UUID PRIMARY KEY,
  name VARCHAR(255) NOT NULL,
  description TEXT,
  price DECIMAL(10, 2) NOT NULL,
  stock_quantity INT DEFAULT 0,
  category_id UUID,
  image_url TEXT,
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW(),
  FOREIGN KEY (category_id) REFERENCES categories(id)
);
```

## 🗂️ 3. Categories Table

Categorize products (e.g., electronics, clothing).

```sql
CREATE TABLE categories (
  id UUID PRIMARY KEY,
  name VARCHAR(255) UNIQUE NOT NULL,
  description TEXT
);
```

## 🛒 4. Cart Items Table

Tracks what users add to their shopping cart.

```sql
CREATE TABLE cart_items (
  id UUID PRIMARY KEY,
  user_id UUID NOT NULL,
  product_id UUID NOT NULL,
  quantity INT NOT NULL,
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW(),
  FOREIGN KEY (user_id) REFERENCES users(id),
  FOREIGN KEY (product_id) REFERENCES products(id)
);
```

## 📦 5. Orders Table

Represents a completed purchase.

```sql
CREATE TABLE orders (
  id UUID PRIMARY KEY,
  user_id UUID NOT NULL,
  status VARCHAR(50) DEFAULT 'pending', -- e.g., pending, shipped, delivered
  total DECIMAL(10, 2) NOT NULL,
  shipping_address TEXT,
  placed_at TIMESTAMP DEFAULT NOW(),
  FOREIGN KEY (user_id) REFERENCES users(id)
);
```

## 📦 6. Order Items Table

Items in each order.

```sql
CREATE TABLE order_items (
  id UUID PRIMARY KEY,
  order_id UUID NOT NULL,
  product_id UUID NOT NULL,
  quantity INT NOT NULL,
  price DECIMAL(10, 2) NOT NULL,
  FOREIGN KEY (order_id) REFERENCES orders(id),
  FOREIGN KEY (product_id) REFERENCES products(id)
);
```

## 💳 7. Payments Table

Tracks payment status and details.

```sql
CREATE TABLE payments (
  id UUID PRIMARY KEY,
  order_id UUID NOT NULL,
  payment_method VARCHAR(100), -- e.g., card, PayPal, cash
  payment_status VARCHAR(50) DEFAULT 'pending',
  paid_at TIMESTAMP,
  FOREIGN KEY (order_id) REFERENCES orders(id)
);
```

## 🧾 Optional: Add Audit Fields & More

You can add audit logs, wishlists, product reviews, delivery tracking, discount codes, etc., depending on your app scope.
