# Ecommerce API Project

A simple **ecommerce** REST API built with Flask, SQLAlchemy, and Marshmallow that manages users, products, and orders in a MySQL database. This project was created as part of a module assignment to practice database design, API development, and request validation.

Repository: `https://github.com/pperez90-lab/ecommerce_API_Project.git`  
Main app file: `app.py`

---

## ✨ Features

- **User management**: Create, read, update, and delete users.
- **Product management**: Create, read, update, and delete products.
- **Order management**:
  - One **User → Many Orders**.
  - Many **Orders ↔ Products** via an association table.
- **Duplicate prevention**: Enforces that the same product cannot be added twice to the same order.
- **Validation & serialization** with Marshmallow.
- **MySQL persistence**, testable via Postman and MySQL Workbench.

---

## 🛠 Tech Stack

- **Language**: Python 3
- **Framework**: Flask
- **ORM**: Flask-SQLAlchemy
- **Serialization/Validation**: Flask-Marshmallow, marshmallow-sqlalchemy
- **Database**: MySQL (via `mysql-connector-python`)
- **Tools**: Postman, MySQL Workbench

---

## 📁 Project Structure

All core logic for this assignment is contained in `app.py`:

- **Models**

  - `User` – `id`, `name`, `address`, `email` (unique), relationship to `orders`.
  - `Order` – `id`, `order_date` (`DateTime`), `user_id` (FK to `User`), relationship to `order_products`.
  - `Product` – `id`, `product_name`, `price` (`Float`), relationship to `order_products`.
  - `OrderProduct` – association table with composite primary key (`order_id`, `product_id`) to prevent duplicates per order.

- **Schemas**

  - `UserSchema` – validates `name`, `email`, `address`.
  - `ProductSchema` – validates `product_name` and `price`.
  - `OrderSchema` – handles `DateTime` `order_date` and includes `user_id` (via `include_fk = True`).

- **Routes**
  - User CRUD: `/users`
  - Product CRUD: `/products`
  - Orders and order-product operations:
    - `/orders`
    - `/orders/<order_id>/add_product/<product_id>`
    - `/orders/<order_id>/remove_product/<product_id>`
    - `/orders/user/<user_id>`
    - `/orders/<order_id>/products`

---

## 🚀 Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/pperez90-lab/ecommerce_API_Project.git
cd ecommerce_API_Project


### 2.Create and activate a virtual environment

python -m venv venv

# Windows
venv\Scripts\activate

# Mac/Linux
source venv/bin/activate


### 3. Install dependencies

pip install Flask Flask-SQLAlchemy Flask-Marshmallow marshmallow-sqlalchemy mysql-connector-python


### 4. Configure MySQL


-- Create the database in MySQL Workbench:

CREATE DATABASE ecommerce_api;


Ensure the connection details in app.py match your local MySQL setup:

app.config['SQLALCHEMY_DATABASE_URI'] = 'mysql+mysqlconnector://<USER>:<PASSWORD>@localhost/ecommerce_api'


### 5. Run the application

python app.py

The API will be available at:
http://localhost:5000


### 📡 API Endpoints

# Users

- GET /users – Retrieve all users.

- GET /users/<id> – Retrieve a user by ID.

- POST /users – Create a new user.

- PUT /users/<id> – Update a user by ID.

- DELETE /users/<id> – Delete a user by ID.



Example – POST /users

json
{
  "name": "Alice",
  "email": "alice@example.com",
  "address": "123 Main St"
}
Products
GET /products – Retrieve all products.

GET /products/<id> – Retrieve a product by ID.

POST /products – Create a new product.

PUT /products/<id> – Update a product by ID.

DELETE /products/<id> – Delete a product by ID.

Example – POST /products

json
{
  "product_name": "Keyboard",
  "price": 49.99
}
Orders
POST /orders – Create a new order (requires user_id and order_date).

PUT /orders/<order_id>/add_product/<product_id> – Add a product to an order (prevents duplicates).

DELETE /orders/<order_id>/remove_product/<product_id> – Remove a product from an order.

GET /orders/user/<user_id> – Get all orders for a specific user.

GET /orders/<order_id>/products – Get all products associated with an order.

Example – POST /orders

json
{
  "order_date": "2025-01-01T10:00:00",
  "user_id": 1
}
Note: user_id must reference an existing user in the users table due to the foreign key constraint.

### 🧪 Testing with Postman
For this assignment, each endpoint is tested and saved as a request in a Postman collection.

Typical workflow:

Start the Flask app:

bash
python app.py
In Postman:

Create a new collection named ecommerce_api.

Add one request per endpoint (users, products, orders).

- For POST and PUT requests, set:

- Header: Content-Type: application/json.

- Body: raw JSON (examples above).

- After creating all requests:

- Click the three dots next to the collection.

- Choose Export → Collection v2.1.

- Save the exported .json file into the project folder (for example: ecommerce_api.postman_collection.json).

###📚 What I Learned
- Working on this project helped me practice:

- Designing relational models with one-to-many and many-to-many relationships in SQLAlchemy.

- Building RESTful endpoints in Flask with clear CRUD semantics.

- Using Marshmallow and Flask-Marshmallow for input validation and JSON serialization.

- Handling foreign key constraints and integrity errors in MySQL.

- Testing APIs with Postman and verifying stored data with MySQL Workbench.

### 🔗 Repository
### GitHub repository:


- https://github.com/pperez90-lab/ecommerce_API_Project.git

```
