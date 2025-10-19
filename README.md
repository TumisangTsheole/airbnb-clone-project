# airbnb-clone-project
The Airbnb Clone Project is a comprehensive, real-world application designed to simulate the development of a robust booking platform like Airbnb. It involves a deep dive into full-stack development, focusing on backend systems, database design, API development, and application security.

The backend for the Airbnb Clone project is designed to provide a robust and scalable foundation for managing user interactions, property listings, bookings, and payments. This backend will support various functionalities required to mimic the core features of Airbnb, ensuring a smooth experience for users and hosts.

# Project Goals
User Management: Implement a secure system for user registration, authentication, and profile management.
Property Management: Develop features for property listing creation, updates, and retrieval.
Booking System: Create a booking mechanism for users to reserve properties and manage booking details.
Payment Processing: Integrate a payment system to handle transactions and record payment details.
Review System: Allow users to leave reviews and ratings for properties.
Data Optimization: Ensure efficient data retrieval and storage through database optimizations.

# Team Roles
Backend Developer: Responsible for implementing API endpoints, database schemas, and business logic.

Database Administrator: Manages database design, indexing, and optimizations.

DevOps Engineer: Handles deployment, monitoring, and scaling of the backend services.

QA Engineer: Ensures the backend functionalities are thoroughly tested and meet quality standards.

# Technology Stack
Django: A high-level Python web framework used for building the RESTful API.

Django REST Framework: Provides tools for creating and managing RESTful APIs.

PostgreSQL: A powerful relational database used for data storage.

GraphQL: Allows for flexible and efficient querying of data.

Celery: For handling asynchronous tasks such as sending notifications or processing payments.

Redis: Used for caching and session management.

Docker: Containerization tool for consistent development and deployment environments.

CI/CD Pipelines: Automated pipelines for testing and deploying code changes.

## Database Design 💾

The application's data is managed using **PostgreSQL**, a powerful relational database. The structure is built around key entities representing the core components of the property rental platform: user management, property listings, bookings, payments, and reviews.

# Key Entities and Fields

**1. Users**
* **Fields:** `id` (Primary Key), `username`, `email`, `role` (Host, Guest, Admin), `join_date`
* **Description:** Stores user account information for authentication and defining roles (who can list properties vs. who can book them).

**2. Properties**
* **Fields:** `id` (Primary Key), `host_id` (Foreign Key to Users), `title`, `address`, `price_per_night`, `status` (Available, Booked)
* **Description:** Represents the rental listings, linking each property back to its host.

**3. Bookings**
* **Fields:** `id` (Primary Key), `guest_id` (Foreign Key to Users), `property_id` (Foreign Key to Properties), `start_date`, `end_date`, `total_price`, `status` (Confirmed, Pending)
* **Description:** Manages reservations, tracking which guest booked which property and for what duration.

**4. Reviews**
* **Fields:** `id` (Primary Key), `booking_id` (Foreign Key to Bookings), `reviewer_id` (Foreign Key to Users), `rating` (1-5), `comment`
* **Description:** Stores user feedback and ratings, ensuring a review is tied to a completed booking.

**5. Payments**
* **Fields:** `id` (Primary Key), `booking_id` (Foreign Key to Bookings), `amount`, `payment_method`, `transaction_status` (Succeeded, Failed)
* **Description:** Records the financial transactions associated with each booking.

---

### Entity Relationships

The entities are connected using **Foreign Keys (FKs)** to enforce data integrity and model real-world relationships, primarily through one-to-many ($\text{1:M}$) links.

* **Users** $\text{1:M}$ **Properties**: A **Host (User)** can list **multiple Properties**.
    * *Mechanism:* The `host_id` field in the **Properties** table links to the `id` in the **Users** table.

* **Users** $\text{1:M}$ **Bookings**: A **Guest (User)** can create **multiple Bookings**.
    * *Mechanism:* The `guest_id` field in the **Bookings** table links to the `id` in the **Users** table.

* **Properties** $\text{1:M}$ **Bookings**: A single **Property** can have **multiple Bookings** over time.
    * *Mechanism:* The `property_id` field in the **Bookings** table links to the `id` in the **Properties** table.

* **Bookings** $\text{1:1}$ **Payments**: Each **Booking** must have a corresponding **Payment** record.
    * *Mechanism:* The `booking_id` field in the **Payments** table links to the `id` in the **Bookings** table.

* **Bookings** $\text{1:M}$ **Reviews**: A completed **Booking** is required before a **Review** can be left.
    * *Mechanism:* The `booking_id` field in the **Reviews** table links to the `id` in the **Bookings** table.
