# Module 0001: **Introduction to Databases and SQL Server**

> **Module Objective**
> To provide learners with a comprehensive understanding of database fundamentals, SQL Server architecture, modeling techniques (ERD, DFD), and the tools required for database development and management.

## Learning Outcomes

By the end of this module, learners will be able to:

1. Explain the fundamental concepts of relational and non-relational databases.
2. Design and interpret ERDs and DFDs.
3. Describe SQL Server architecture and its key components.
4. Identify various SQL Server editions and related tools.
5. Compare SQL Server with other popular database platforms.
6. Install SQL Server and explore its environment.

## Reference Materials

- **Books:**

  - "Database System Concepts | 6th Edition" by Silberschatz, Korth, and Sudarshan
  - "Microsoft SQL Server 2019: A Beginner's Guide, Seventh Edition" by Dusan Petkovic

- **Online Resources:**

  - Microsoft Learn: What is sql server ([https://learn.microsoft.com/en-us/sql/sql-server/what-is-sql-server?view=sql-server-ver17](https://learn.microsoft.com/en-us/sql/sql-server/what-is-sql-server?view=sql-server-ver17))
  - Azure SQL Documentation ([https://learn.microsoft.com/en-us/azure/azure-sql/](https://learn.microsoft.com/en-us/azure/azure-sql/))

## Key Concepts & Detailed Content

### 1. **Introduction to Databases**

- **Definition**: A database is a collection of interrelated data and programs designed to access and modify that data.
- **Types**:
  - **Relational (SQL Server, MySQL, PostgreSQL)**: These are the most common type, organizing data into one or more tables (or "relations") of rows and columns. Each row is a record, and each column represents an attribute. Relationships between tables are established using common fields. They use SQL (Structured Query Language) for defining and manipulating data.
  - **NoSQL (MongoDB, Cassandra)**: These databases provide a mechanism for storage and retrieval of data that is modeled in means other than the tabular relations used in relational databases. They are often used for handling large sets of distributed data and are popular in big data and real-time web applications.
  - **Hierarchical (IBM Information Management System (IMS). While less common in new development, they are still used in legacy systems.)**: An older type of database where data is organized in a tree-like structure. Data is stored as "records" that are connected to one another through "links," resembling a one-to-many relationship (parent-child).
- **Core Elements**:

  - **Entities**: An entity is a "thing" or "object" in the real world that is distinct from all other objects. Entities can be concrete, such as a person or a book, or abstract, like a course offering or a flight reservation. An entity set is a collection of entities of the same type that share similar properties or attributes. For example, "instructor" could be an entity set representing all instructors at a university.
  - **Attributes**: Attributes are descriptive properties that each member of an entity set possesses. They define the characteristics of an entity. For instance, an instructor entity set might have attributes like ID, name, dept_name, and salary. Each entity will have its own specific value for each attribute.
  - **Relationships**: Relationships describe how entities are connected or associated with each other. For example, in a university database, a student entity might "take" a course offering entity, and an instructor entity "teaches" a course offering. These "takes" and "teaches" are examples of relationships between entities. The Entity-Relationship (E-R) model helps in mapping these real-world meanings and interactions into a conceptual schema for the database.

- **ACID Properties**: ACID is an acronym for a set of properties that guarantee valid and reliable database transactions. These properties are crucial for ensuring data integrity, especially in concurrent environments.
  - **Atomicity**: Ensures that a transaction is treated as a single, indivisible unit of work. Either all of the operations within the transaction are completed successfully, or none of them are. If any part of the transaction fails, the entire transaction is rolled back, leaving the database in its state before the transaction began. (e.g., in a money transfer, either both deduction from one account and addition to another happen, or neither does).
  - **Consistency**: Ensures that a transaction brings the database from one valid state to another valid state. All data must follow predefined rules, constraints, and cascades. If a transaction violates any rules, it's rolled back. (e.g., if a database rule says account balances must be positive, a transaction that would make it negative will be rejected).
  - **Isolation**: Guarantees that concurrent transactions execute in isolation from each other. The intermediate state of a transaction is not visible to other transactions until the transaction is committed. This prevents anomalies that can occur when multiple transactions try to access and modify the same data simultaneously. (e.g., two users trying to book the last available seat on a flight will not interfere with each other, and only one will succeed).
  - **Durability**: Ensures that once a transaction has been committed, it will remain permanent, even in the event of system failures (like power outages or crashes). Committed data is written to non-volatile storage and will survive any subsequent system restart. (e.g., once a payment is confirmed, it will not disappear from the records even if the server crashes right after).

### 2. **Database Design Using ER Diagrams**

Database design using Entity-Relationship (ER) Diagrams is a crucial step in creating efficient and well-structured databases. It involves identifying and modeling the key components of the system.

- **Entity Types**:

  - **Strong (Independent) Entity**: An entity that can exist on its own and has a unique primary key. It does not depend on another entity for its existence. For example, a "Customer" or a "Product" in a shopping cart system.
  - **Weak (Dependent) Entity**: An entity whose existence depends on another (strong) entity. It does not have a primary key of its own; its primary key is derived in part from the strong entity it depends on. For example, "Payment Details" for a specific order, where the payment details wouldn't exist without the order.

- **Attributes**:

  - **Simple**: An attribute that cannot be broken down into smaller, meaningful parts. For example, "Name" or "Price".
  - **Composite**: An attribute that can be divided into several simpler attributes. For example, "Address" could be composed of "Street", "City", and "Zip Code".
  - **Derived**: An attribute whose value can be calculated from other attributes and is not stored directly in the database. For example, "Total Order Amount" can be derived from the sum of prices of all items in an order.

- **Relationships**:

  - **One-to-One (1:1)**: Each entity in one set is related to at most one entity in another set. For example, a "Customer" might have one "Shopping Cart" at a time.
  - **One-to-Many (1:N)**: One entity in the first set can be related to multiple entities in the second set, but each entity in the second set is related to at most one entity in the first set. For example, one "Order" can have many "Order Items".
  - **Many-to-Many (M:N)**: Entities in both sets can be related to multiple entities in the other set. For example, many "Customers" can "buy" many "Products", and many "Products" can be "bought" by many "Customers".

- **Notation and Symbols**:
  Various graphical notations are used to draw ER Diagrams, helping to visually represent the entities, attributes, and relationships. Common ones include:

  - **Crow’s Foot Notation**: Popular for its clear representation of cardinality (the "one" or "many" side of a relationship) using symbols that resemble a crow's foot.

    ```mermaid
     erDiagram
        A ||--o| B : "One or Zero"
        C ||--|| D : "One and only One"
        E ||--o{ F : "Zero or Many"
        G ||--|{ H : "One or Many"
    ```

    #### `A ||--o| B : "One or Zero"`

    - **Left (A)**: Exactly **one** (`||`)
    - **Right (B)**: **Optional** (zero or one) (`o|`)
    - **Meaning**: One `A` is associated with **zero or one** `B`

    #### `C ||--|| D : "One and only One"`

    - **Both sides**: Exactly **one** (`||`)
    - **Meaning**: One `C` is associated with exactly **one** `D`, and vice versa — a **1:1 relationship**

    #### `E ||--o{ F : "Zero or Many"`

    - **Left (E)**: Exactly **one** (`||`)
    - **Right (F)**: **Zero or many** (`o{`)
    - **Meaning**: One `E` can be associated with **zero, one, or many** `F`s — a **1\:N (optional)** relationship

    #### `G ||--|{ H : "One or Many"`

    - **Left (G)**: Exactly **one** (`||`)
    - **Right (H)**: **One or many** (`|{`)
    - **Meaning**: One `G` must be associated with **at least one** `H` — a **1\:N (mandatory)** relationship

  - **UML (Unified Modeling Language)**: While primarily used for object-oriented design, UML class diagrams can be adapted to model ER concepts.
  - **Chen Notation**: An older, more traditional notation that uses rectangles for entities, diamonds for relationships, and ovals for attributes.

#### 2.1. This ER Diagram below models a simplified e-commerce or shopping cart system. Let's break down its components:

```mermaid
erDiagram
    CUSTOMER ||--o{ ORDER : places
    CUSTOMER ||--|| SHOPPING_CART : has
    CUSTOMER }|--|{ PRODUCT : buys
    ORDER ||--o{ ORDER_ITEM : consists_of
    ORDER ||--o{ PAYMENT_DETAILS : has
    PRODUCT ||--o{ ORDER_ITEM : contains

    CUSTOMER {
        int CustomerID PK
        varchar Name
        varchar Email
        varchar Street "Composite: CustomerAddress"
        varchar City "Composite: CustomerAddress"
        varchar ZipCode "Composite: CustomerAddress"
    }

    PRODUCT {
        int ProductID PK
        varchar Name
        decimal Price
    }

    ORDER {
        int OrderID PK
        date OrderDate
        decimal Total_Order_Amount "Derived"
    }

    PAYMENT_DETAILS {
        int TransactionID PK
        varchar PaymentMethod
        decimal Amount
    }

    SHOPPING_CART {
        int CartID PK
        date CreationDate
    }

    ORDER_ITEM {
        int ItemID PK
        int OrderID FK
        int ProductID FK
        int Quantity
    }
```

- **CUSTOMER**: Represents individuals making purchases.
  - `CustomerID` (PK): Unique identifier for each customer.
  - `Name`, `Email`: Simple attributes storing customer information.
  - `Street`, `City`, `ZipCode`: These collectively form a **Composite Attribute** named "CustomerAddress," although individually listed here.
- **PRODUCT**: Represents items available for purchase.
  - `ProductID` (PK): Unique identifier for each product.
  - `Name`, `Price`: Simple attributes describing the product.
- **ORDER**: Represents a customer's purchase.
  - `OrderID` (PK): Unique identifier for each order.
  - `OrderDate`: Simple attribute indicating when the order was placed.
  - `Total_Order_Amount` (Derived): This is a **Derived Attribute**, meaning its value is calculated from the prices and quantities of the products within the order, rather than being stored directly.
- **PAYMENT_DETAILS**: Represents the payment information for an order.
  - `TransactionID` (PK): Unique identifier for a payment transaction.
  - `PaymentMethod`, `Amount`: Simple attributes.
  - **Weak Entity**: This entity is likely a weak entity, as specific payment details usually depend on the existence of an `ORDER`. The relationship `ORDER ||--o{ PAYMENT_DETAILS` suggests this dependency.
- **SHOPPING_CART**: Represents a customer's current shopping cart.
  - `CartID` (PK): Unique identifier for a shopping cart.
  - `CreationDate`: Simple attribute.
- **ORDER_ITEM**: Represents a specific product included in an order. This often acts as a "bridge" or "junction" entity to resolve many-to-many relationships.
  - `ItemID` (PK): Unique identifier for each item within an order.
  - `OrderID` (FK): Foreign key linking to the `ORDER` it belongs to.
  - `ProductID` (FK): Foreign key linking to the `PRODUCT` being ordered.
  - `Quantity`: Simple attribute indicating how many units of the product are in this order item.

#### 2.2 Relationships and Cardinality:

- **CUSTOMER ||--o{ ORDER : places**
  - **One-to-Many (1:N)**: A `CUSTOMER` can `places` many `ORDER`s, but each `ORDER` is placed by only one `CUSTOMER`. The `||--o{` notation means "exactly one" on the left side (customer) and "zero or many" on the right side (order).
- **CUSTOMER ||--|| SHOPPING_CART : has**
  - **One-to-One (1:1)**: A `CUSTOMER` `has` exactly one `SHOPPING_CART`, and a `SHOPPING_CART` belongs to exactly one `CUSTOMER`.
- **CUSTOMER }|--|{ PRODUCT : buys**
  - **Many-to-Many (M:N)**: Many `CUSTOMER`s can `buys` many `PRODUCT`s, and many `PRODUCT`s can be `bought` by many `CUSTOMER`s. The `}|--|{` notation signifies "many" on both sides. In a relational database, this relationship is typically resolved by an intermediary table, which in our diagram is `ORDER_ITEM`.
- **ORDER ||--o{ ORDER_ITEM : consists_of**
  - **One-to-Many (1:N)**: An `ORDER` `consists_of` many `ORDER_ITEM`s, but each `ORDER_ITEM` belongs to only one `ORDER`.
- **ORDER ||--o{ PAYMENT_DETAILS : has**
  - **One-to-Many (1:N)**, specifically representing a **Weak Entity Relationship**: An `ORDER` `has` many `PAYMENT_DETAILS` (though often it's one order to one payment, this notation allows for multiple payments or payment attempts). The `o{` on the `PAYMENT_DETAILS` side also implies it might be a weak entity, relying on `ORDER` for its existence.
- **PRODUCT ||--o{ ORDER_ITEM : contains**
  - **One-to-Many (1:N)**: A `PRODUCT` can be `contains` in many `ORDER_ITEM`s, but each `ORDER_ITEM` `contains` only one `PRODUCT`. This relationship, along with `ORDER ||--o{ ORDER_ITEM`, shows how `ORDER_ITEM` acts as the bridge for the many-to-many relationship between `ORDER` and `PRODUCT`.

### 3. **System Analysis Using Data Flow Diagrams (DFDs)**

Data Flow Diagrams (DFDs) are a fundamental tool in system analysis, used to visually represent how data moves through a system. In the context of a Shopping Cart System, DFDs help us understand the flow of information from customers, through various system processes, and into data storage.

- **DFD Levels**: DFDs are often structured in hierarchical levels to provide increasing detail.

  - **Context Diagram (Level 0)**: This is the highest-level DFD, representing the entire Shopping Cart System as a single process. It shows the system's interaction with external entities (sources and sinks of data). For our Shopping Cart System, the primary external entity would be the `Customer`. Data flows would include `Customer Request`, `Product Information`, `Order Details`, `Payment Confirmation`, etc.
  - **Level-1 (Detailed) DFD**: This level breaks down the single process from the context diagram into its major sub-processes. For a Shopping Cart System, these might include:
    - `Browse Products`
    - `Manage Shopping Cart`
    - `Place Order`
    - `Process Payment`
    - `Manage Customer Accounts`
      It would show how data flows between these sub-processes, data stores (which correspond to the entities in your ERD), and external entities.

- **Elements**: DFDs use a specific set of symbols to represent different components:

  - **Processes (Circles or Rounded Rectangles)**: Transform incoming data flows into outgoing data flows. Examples in a shopping cart system: `Add Item to Cart`, `Calculate Order Total`, `Verify Payment`.
  - **Data Stores (Open Rectangles or Parallel Lines)**: Represent a place where data is held or stored within the system. These directly correspond to the entities from your ERD: `Customer` (Data Store for Customer information), `Product` (Data Store for product catalog), `Order` (Data Store for order history), `Payment_Details` (Data Store for payment records), and `Shopping_Cart` (Data Store for active shopping carts), `Order_Item` (Data Store for individual items within orders).
  - **Data Flows (Arrows)**: Represent the movement of data between processes, data stores, and external entities. Examples: `Product Search Query` (from Customer to Browse Products), `Added Item Details` (from Manage Shopping Cart to Shopping_Cart Data Store), `Order Confirmation` (from Place Order to Customer).
  - **External Entities (Squares)**: Represent sources or destinations of data outside the system's boundaries. For the Shopping Cart System, the primary external entity is the `Customer`. Other potential external entities could be a `Payment Gateway` (for processing payments) or a `Warehouse/Fulfillment System` (for shipping orders).

- **Mapping**: After designing DFDs, the information gathered can be used to inform the database design, particularly for converting DFD elements into relational schema components:

  - **Data Stores** directly map to **tables** in a relational database. For instance, the `Customer` data store would become the `Customer` table, the `Product` data store would become the `Product` table, and so on.
  - **Data Flows** help identify the attributes needed within these tables and the relationships between them. For example, a "Product Details" data flow to a "Display Products" process would suggest that the `Product` table needs attributes like `Name`, `Price`, and `Description`.
  - **Processes** help define the operations (e.g., SQL queries, stored procedures, application logic) that will be performed on the data within the tables to fulfill the system's functions (e.g., adding an item, placing an order, processing a payment).

- **Case Study: Design DFD for Shopping Cart System**:

  - **DFD**:

    - **Context Diagram (Level 0)**:
      - **External Entity**: `Customer`
      - **Process**: `Shopping Cart System`
      - **Data Flows**: `Customer Request`, `Product Catalog`, `Order Information`, `Payment Info`, `Confirmation`

    ```mermaid
    graph TD
        A[Customer] -->|Customer Request| B(Shopping Cart System)
        B -->|Product Catalog, Order Info, Payment Conf., Status Messages| A
    ```

    - **Explanation of Level 0 DFD:**

      - **External Entity**: `Customer` (represented by a square `A`) is the primary external entity interacting with the system.
      - **Process**: `Shopping Cart System` (represented by a rounded rectangle `B`) is the single process representing the entire system.
      - **Data Flows**:
        - The `Customer` sends `Customer Request` to the `Shopping Cart System`.
        - The `Shopping Cart System` sends back `Product Catalog`, `Order Info`, `Payment Conf.`, and `Status Messages` to the `Customer`.

    - **Level 1 DFD (Detailed Diagram)**

    ```mermaid
    graph TD
        subgraph Shopping Cart System
            direction LR
            P1(Browse Products)
            P2(Manage Shopping Cart)
            P3(Checkout & Place Order)
            P4(Process Payment)
            P5(Manage Customer Account)

            DS1[Customer Data Store]
            DS2[Product Data Store]
            DS3[Shopping Cart Data Store]
            DS4[Order Data Store]
            DS5[Order Item Data Store]
            DS6[Payment Details Data Store]
        end

        A[Customer] -->|Product Search Query| P1
        P1 -->|Product Request| DS2
        DS2 -->|Product Details| P1
        P1 -->|Product Catalog Display| A

        A -->|Add/Remove Item Request| P2
        A -->|Update Quantity| P2
        P2 -->|Cart Updates| DS3
        DS3 -->|Current Cart Contents| P2
        P2 -->|Cart Confirmation| A

        A -->|Checkout Request| P3
        A -->|Shipping Info| P3
        A -->|Initial Payment Info| P3
        P3 -->|Get Final Cart| DS3
        P3 -->|New Order Details| DS4
        P3 -->|Order Item Details| DS5
        P3 -->|Payment Request| P4
        P4 -->|Payment Request| B[Payment Gateway]
        B -->|Payment Response| P4
        P4 -->|Save Payment Record| DS6
        P4 -->|Payment Status| P3
        P3 -->|Order Confirmation| A
        P3 -->|Payment Status to Customer| A

        A -->|Account Login| P5
        A -->|Profile Update Request| P5
        P5 -->|Customer Account Updates| DS1
        DS1 -->|Customer Profile| P5
        P5 -->|Profile Update Conf/Error| A

        DS1 -- Customer Info --> P3
        DS2 -- Product Price Lookup --> P3
        DS4 -- Order History Request --> P5
        P5 -- Order History Details --> DS4
    ```

    - **Explanation of Level 1 DFD:**

      - **External Entities**:
        - `Customer` (represented by `A`) remains the primary external entity.
        - `Payment Gateway` (represented by `B`) is introduced as another external entity that interacts specifically with the payment processing.
      - **Processes**: The single "Shopping Cart System" is now broken down into five main sub-processes:
        - `Browse Products` (`P1`): Handles product search and display.
        - `Manage Shopping Cart` (`P2`): Manages adding, removing, and updating items in the cart.
        - `Checkout & Place Order` (`P3`): Orchestrates the finalization of an order, including shipping and initial payment details.
        - `Process Payment` (`P4`): Handles the actual payment transaction.
        - `Manage Customer Account` (`P5`): Deals with customer login and profile management.
      - **Data Stores**: These correspond directly to the entities identified in your ER Diagram:
        - `Customer Data Store` (`DS1`)
        - `Product Data Store` (`DS2`)
        - `Shopping Cart Data Store` (`DS3`)
        - `Order Data Store` (`DS4`)
        - `Order Item Data Store` (`DS5`)
        - `Payment Details Data Store` (`DS6`)
      - **Data Flows**: The arrows show the movement of specific data between these processes, data stores, and external entities. For example:
        - A `Customer` sends a `Product Search Query` to `Browse Products`.
        - `Browse Products` retrieves `Product Details` from the `Product Data Store` and displays the `Product Catalog` to the `Customer`.
        - When an order is placed, `Checkout & Place Order` sends `Payment Request` to `Process Payment`, which then interacts with the `Payment Gateway` to process the transaction and updates the `Payment Details Data Store`.
        - The diagram also shows how customer account information is managed and updated.

### 4. **Data Structures & Normalization**

Understanding how data is structured and organized within a database is crucial for its efficiency, integrity, and maintainability. This section covers the fundamental concepts of table anatomy, constraints, and the various levels of normalization.

- **Table Anatomy**:

  - **Rows (Records/Tuples)**: A single entry in a table, representing a single instance of the entity. For example, in a `CUSTOMER` table, each row would represent a unique customer.
  - **Columns (Fields/Attributes)**: Represent a specific piece of information or property about the entity. Each column has a unique name within the table. For example, in a `CUSTOMER` table, `CustomerID`, `Name`, and `Email` would be columns.
  - **Data Types**: Define the type of data that can be stored in a column (e.g., `INT` for integers, `VARCHAR` for variable-length strings, `DATE` for dates, `DECIMAL` for numbers with decimal places). Choosing appropriate data types ensures data integrity and optimizes storage.

- **Constraints**: Rules enforced on data columns to limit the type of data that can be entered, ensuring data accuracy and reliability.

  - `NOT NULL`: Ensures that a column cannot have a `NULL` (empty) value. For example, a `CustomerID` should always have a value.
  - `UNIQUE`: Ensures that all values in a column (or a group of columns) are distinct. For example, `Email` in a `CUSTOMER` table might be `UNIQUE` to prevent duplicate email registrations.
  - `DEFAULT`: Provides a default value for a column when no value is explicitly specified during data insertion. For example, `OrderDate` could default to the current date.
  - `CHECK`: Enforces a condition that all values in a column must satisfy. For example, a `Price` column might have a `CHECK` constraint to ensure values are always greater than 0.
  - `PRIMARY KEY`: Uniquely identifies each row in a table. It must contain `UNIQUE` values and cannot contain `NULL` values. A table can have only one `PRIMARY KEY`. For example, `CustomerID` in the `CUSTOMER` table.
  - `FOREIGN KEY`: A field (or collection of fields) in one table that refers to the `PRIMARY KEY` in another table. It establishes a link between two tables, enforcing referential integrity. For example, `OrderID` in the `ORDER_ITEM` table would be a `FOREIGN KEY` referencing the `OrderID` in the `ORDER` table.

- **Normalization**: A systematic process of structuring a relational database in order to minimize data redundancy and improve data integrity. It involves breaking down large tables into smaller, less redundant tables and defining relationships between them.

  - **1NF (First Normal Form): Atomicity**:
    - Each column must contain only atomic (indivisible) values. This means no repeating groups or multi-valued attributes within a single cell.
    - Each row must be unique (have a primary key).
  - **2NF (Second Normal Form): Partial Dependency Removal**:
    - Must be in 1NF.
    - All non-key attributes (attributes not part of the primary key) must be fully dependent on the _entire_ primary key. If the primary key is composite (made of multiple columns), no non-key attribute should depend on only a part of it.
  - **3NF (Third Normal Form): Transitive Dependency Removal**:
    - Must be in 2NF.
    - No non-key attribute should be transitively dependent on the primary key. This means no non-key attribute should depend on another non-key attribute. For example, if `A` determines `B`, and `B` determines `C`, then `A` transitively determines `C`. In 3NF, `C` should directly depend on `A` or be moved to a separate table.
  - **BCNF (Boyce-Codd Normal Form)**:
    - Must be in 3NF.
    - Every determinant must be a candidate key. A determinant is any attribute or set of attributes on which some other attribute is functionally dependent. BCNF is a stricter form of 3NF, addressing certain anomalies that 3NF might miss, especially with composite keys and overlapping candidate keys.
  - **4NF (Fourth Normal Form)**:
    - Must be in BCNF.
    - No multi-valued dependencies. A multi-valued dependency occurs when there are two or more independent multi-valued attributes in the same table that are dependent on the same determinant.
  - **5NF (Fifth Normal Form / Project-Join Normal Form)**:
    - Must be in 4NF.
    - No join dependency. This means a table cannot be decomposed into smaller tables without losing information, where the original table can be reconstructed by joining the smaller tables. It eliminates redundancy that can arise from having multiple overlapping candidate keys.

- **Denormalization**: A strategy used to intentionally introduce redundancy into a database by combining tables or adding derived data, typically to improve query performance. This is often done after normalization, when specific performance bottlenecks are identified. While it can speed up read operations (queries), it might increase data redundancy and complexity in managing data consistency during write operations.

### 5. **Overview of SQL Server**

Microsoft SQL Server is a powerful relational database management system (RDBMS) designed to store, retrieve, and manage data efficiently. It's widely used across industries for a variety of data-driven applications. To understand its capabilities, it helps to explore key concepts, editions, typical use cases, and licensing models.

- **What is an RDBMS?**

  - **RDBMS stands for Relational Database Management System.** It organizes data into tables (also called relations) with rows and columns.
  - The relational model was first proposed by **E.F. Codd** in 1970.
  - RDBMSs enable structured, consistent, and easily queryable data storage.
  - SQL Server uses SQL (Structured Query Language) to query and manage relational data.

- **SQL Server Editions**
  Microsoft offers several editions of SQL Server, tailored to different needs:

  - **Enterprise Edition**: Feature-rich and designed for mission-critical applications, large-scale data warehousing, and high-throughput OLTP workloads. Includes advanced security, availability, and performance features.
  - **Standard Edition**: Provides core database capabilities suitable for most mid-tier applications and small-scale data warehouses. More cost-effective than Enterprise, but with feature limitations.
  - **Developer Edition**: Identical in features to Enterprise Edition, but licensed only for development and testing, not for production. Free for developers to use in non-production environments.
  - **Express Edition**: A free, lightweight edition with limitations on database size, memory, and CPU usage. Ideal for learning, small web apps, or desktop-based development.

- **Common Use Cases**
  SQL Server is used in a wide range of scenarios:

  - **OLTP (Online Transaction Processing)**: Handles high volumes of short, fast transactions (e.g., e-commerce, banking, POS systems). Emphasis on speed, concurrency, and data integrity.
  - **Data Warehousing**: Supports large-scale data aggregation and analysis. Used to build centralized repositories for reporting and analytics (often using SQL Server Analysis Services).
  - **Business Intelligence (BI)**: SQL Server includes tools like SSRS (for reporting) and SSAS (for analytics) to enable advanced insights and dashboards. Helps businesses analyze trends, forecast performance, and make data-driven decisions.

- **Licensing Models**
  Microsoft provides two main licensing models for SQL Server:
  - **Core-Based Licensing**: Licenses are based on the number of physical or virtual cores. Suitable for environments with heavy workloads, virtualization, or cloud deployment. Minimum: 4 core licenses per physical processor.
  - **Server + CAL Licensing (Client Access License)**: Requires a license for the SQL Server instance and a CAL for each user or device accessing it. More cost-effective for smaller, predictable user counts.

### 6. **SQL Server Architecture**

SQL Server is a robust relational database management system (RDBMS) developed by Microsoft. Its architecture is designed to efficiently store, retrieve, manage, and secure data. Understanding its core components and how a query is processed is fundamental to optimizing its performance and ensuring data integrity.

- **Components**:

  - **Database Engine**: This is the core service of SQL Server. It's responsible for managing all database operations, including data storage, processing, and security. It contains both the Relational Engine (which processes queries) and the Storage Engine (which manages files, pages, and indexing).
  - **Storage Engine**: This component is responsible for the physical data management. It handles all operations related to input/output (I/O) to the database files (data files and log files). Key functions include:
    - Managing how data is stored on disk (pages, extents).
    - Implementing indexing for faster data retrieval.
    - Ensuring data recovery and consistency through logging.
  - **Query Processor (or Relational Engine)**: This component is the brain behind executing T-SQL (Transact-SQL, Microsoft's proprietary extension to SQL) statements. Its main tasks include:
    - **Parsing**: Checking the syntax of the T-SQL query and translating it into an internal representation.
    - **Optimization**: Generating an efficient execution plan for the query. This is a critical step where the optimizer considers various factors (indexes, statistics, joins) to find the fastest way to retrieve the requested data.
    - **Execution**: Running the chosen execution plan to perform the actual data retrieval or modification.
  - **Buffer Pool**: This is the main memory area within SQL Server that acts as a cache for data pages read from disk. When data is requested, the Storage Engine first checks the Buffer Pool. If the data is present (a "cache hit"), it's retrieved much faster than from disk. If not (a "cache miss"), the data is read from disk into the Buffer Pool for future use. Efficient use of the Buffer Pool is vital for performance.
  - **Lock Manager**: In a multi-user environment, multiple users might try to access or modify the same data simultaneously. The Lock Manager is responsible for concurrency control, ensuring that data integrity is maintained. It acquires and releases various types of locks (e.g., shared locks for reads, exclusive locks for writes) on database resources (rows, pages, tables) to prevent conflicts and maintain the ACID properties (Atomicity, Consistency, Isolation, Durability) of transactions.
  - **SQL Server Agent**: A background service that automates and executes scheduled administrative tasks, such as backups, replication, and job execution.
  - **SQL Server Reporting Services (SSRS)**: A platform used for creating, deploying, and managing various types of reports from different data sources.
  - **SQL Server Integration Services (SSIS)**: A platform for building high-performance data integration and workflow solutions. It's commonly used for Extract, Transform, Load (ETL) operations.
  - **SQL Server Analysis Services (SSAS)**: A tool for Online Analytical Processing (OLAP) and data mining, used to create analytical databases (cubes) for business intelligence.

- **Query Lifecycle**: When a T-SQL query is submitted to SQL Server, it goes through a well-defined lifecycle:

  1.  **Protocol Layer**: The query first arrives at the Protocol Layer, which manages the communication between the client application and the SQL Server instance.
  2.  **Parsing**: The query text is parsed by the Query Parser (part of the Relational Engine). This step checks for syntax errors, validates object names (tables, columns), and converts the query into a query tree (a logical representation).
  3.  **Optimization**: The Query Optimizer takes the query tree and generates several possible execution plans. It analyzes statistics, indexes, and existing data to estimate the cost (CPU, I/O) of each plan and selects the most efficient one. This chosen plan dictates how the data will be accessed and processed.
  4.  **Execution**: The Execution Engine then takes the optimized execution plan and performs the actual operations. It interacts with the Storage Engine to retrieve or modify data, utilizing the Buffer Pool for caching and the Lock Manager for concurrency control. The results are then returned to the client.

**Block Diagram of SQL Server Architecture:**

```mermaid
graph TD

    %% Client Applications
    subgraph Client_Applications[Client Applications]
        C1[User Application]
        C2[SQL Server Management Studio]
    end

    C1 --> P[Protocol Layer]
    C2 --> P

    %% SQL Server Core Components
    subgraph SQL_Server_Components[SQL Server Core Components]
        direction TB

        P --> RE[Relational Engine / Query Processor]

        subgraph Relational_Engine_Details[Relational Engine Details]
            QP[Query Parser]
            QO[Query Optimizer]
            QE[Execution Engine]
        end

        RE --> QP --> QO --> QE
        QE --> SE[Storage Engine]

        RE --> BP[Buffer Pool]
        RE --> LM[Lock Manager]
        RE --> TM[Transaction Manager]

        SE --> DAF[Data Files]
        SE --> LF[Log Files]

        BP -- manages --> DAF
        LM -- controls access --> DAF
        TM -- ensures ACID --> LF
    end

    %% SQL Server Services
    subgraph SQL_Server_Services[SQL Server Services]
        SAS[SQL Server Agent]
        SSRS[SQL Server Reporting Services]
        SSIS[SQL Server Integration Services]
        SSAS[SQL Server Analysis Services]
    end

    RE --- SAS
    RE --- SSRS
    RE --- SSIS
    RE --- SSAS

    %% Styles
    style P fill:#f9f,stroke:#333,stroke-width:2px
    style RE fill:#cef,stroke:#333,stroke-width:2px
    style SE fill:#fc9,stroke:#333,stroke-width:2px
    style QP fill:#fff,stroke:#333,stroke-width:1px
    style QO fill:#fff,stroke:#333,stroke-width:1px
    style QE fill:#fff,stroke:#333,stroke-width:1px
    style BP fill:#bdf,stroke:#333,stroke-width:2px
    style LM fill:#cfb,stroke:#333,stroke-width:2px
    style TM fill:#fcc,stroke:#333,stroke-width:2px
    style DAF fill:#eee,stroke:#333,stroke-width:1px
    style LF fill:#eee,stroke:#333,stroke-width:1px

```

**Sequence Diagram of SQL Server Query Lifecycle:**

```mermaid
sequenceDiagram
    participant Client
    participant ProtocolLayer as Protocol Layer
    participant RelationalEngine as Relational Engine (Query Processor)
    participant StorageEngine as Storage Engine
    participant BufferPool as Buffer Pool
    participant LockManager as Lock Manager
    participant TransactionManager as Transaction Manager

    Client->>ProtocolLayer: T-SQL Query
    ProtocolLayer->>RelationalEngine: Query received
    RelationalEngine->>RelationalEngine: Parse Query (Syntax, Object Validation)
    RelationalEngine->>RelationalEngine: Optimize Query (Generate Execution Plan)
    RelationalEngine->>RelationalEngine: Execute Plan

    alt Data Access Required (Read/Write)
        RelationalEngine->>StorageEngine: Data Request (using Execution Plan)
        StorageEngine->>BufferPool: Check Cache for Data Page
        alt Cache Hit
            BufferPool-->>StorageEngine: Data Page from Cache
        else Cache Miss
            StorageEngine->>StorageEngine: Read Data Page from Disk
            StorageEngine->>BufferPool: Load Data Page into Cache
        end
        StorageEngine->>LockManager: Request/Acquire Locks
        LockManager-->>StorageEngine: Lock Granted
        StorageEngine-->>RelationalEngine: Data (from Buffer Pool/Disk)
        RelationalEngine->>TransactionManager: Report Transaction Progress/Commit
        TransactionManager-->>StorageEngine: Log Operations (to Log Files)
        StorageEngine->>LockManager: Release Locks
    end

    RelationalEngine-->>ProtocolLayer: Query Results
    ProtocolLayer-->>Client: Results
```

### 7. **System Databases in SQL Server**

| Database   | Description                |
| ---------- | -------------------------- |
| `master`   | System-wide configurations |
| `model`    | Template for new databases |
| `msdb`     | SQL Agent, scheduling      |
| `tempdb`   | Temporary objects          |
| `resource` | Internal system metadata   |

### 8. **SQL Server Tool Ecosystem**

SQL Server is supported by a rich ecosystem of tools that facilitate its management, development, and integration with other systems. These tools cater to different preferences, from graphical user interfaces to command-line utilities, and offer seamless integration with development environments.

- **GUI Tools (Graphical User Interface Tools)**: These tools provide a visual interface for managing and interacting with SQL Server, making it easier for users to perform complex tasks without extensive command-line knowledge.

  - **SSMS (SQL Server Management Studio)**: This is the primary and most comprehensive integrated environment for managing any SQL infrastructure, from SQL Server to Azure SQL Database. SSMS provides tools to configure, monitor, and administer instances of SQL Server and databases. It also includes tools for developing components used with SQL Server, such as queries, views, stored procedures, and more.
  - **Azure Data Studio**: A cross-platform database tool for data professionals using the Microsoft family of on-premises and cloud data platforms on Windows, macOS, and Linux. It offers a modern editor experience with IntelliSense, code snippets, source control integration (Git), and an integrated terminal. It's often preferred for light-weight development and quick query execution across various data sources.

- **CLI Tools (Command-Line Interface Tools)**: For scripting, automation, or environments where a graphical interface is not available or preferred, SQL Server offers robust command-line tools.

  - **`sqlcmd`**: A command-line utility for executing T-SQL statements, scripts, and system procedures. It's particularly useful for automating administrative tasks and running scripts in batch files.
  - **`bcp` (Bulk Copy Program)**: A command-line utility for efficiently copying large amounts of data between a SQL Server instance and a data file in a specified format. It's frequently used for data migration and ETL (Extract, Transform, Load) processes.
  - **PowerShell**: A cross-platform task automation and configuration management framework. SQL Server provides PowerShell cmdlets (command-lets) that allow administrators to manage SQL Server instances, databases, and objects programmatically, making it ideal for advanced scripting and automation.

- **Development Integration**: SQL Server is designed to integrate smoothly with popular development environments and modern development practices.
  - **Visual Studio + SSDT (SQL Server Data Tools)**: Visual Studio is Microsoft's integrated development environment (IDE). SSDT is an extension for Visual Studio that provides tools for developing relational databases, data warehouses, and BI solutions. It allows developers to create, debug, and refactor databases using a declarative, model-based approach.
  - **Git Integration, CI/CD Support**: Modern SQL Server development workflows strongly support integration with version control systems like Git. This enables collaborative development, tracking changes, and reverting to previous versions. Furthermore, SQL Server projects (often created with SSDT) can be integrated into Continuous Integration/Continuous Delivery (CI/CD) pipelines, automating the build, test, and deployment of database changes, ensuring consistent and rapid delivery of updates.

### 9. **SQL Server on Cloud & Linux**

SQL Server has evolved significantly beyond its traditional Windows-only, on-premises deployment, embracing cloud platforms and open-source operating systems. This expansion offers greater flexibility, scalability, and deployment options.

- **Azure SQL Models**: Microsoft Azure, their cloud computing service, offers various models for deploying SQL Server, catering to different levels of management, cost, and control. These are generally categorized by the "as a Service" model:

  - **Azure SQL Database (PaaS - Platform as a Service)**: This is a fully managed, intelligent, and scalable relational database service built on Azure. Microsoft handles all the underlying infrastructure, operating system, and database software updates. Users focus solely on application development and data. It offers high availability, automated backups, and built-time intelligence without the overhead of infrastructure management.
  - **Azure SQL Managed Instance (Hybrid)**: This is a fully managed SQL Server Database Engine instance in the Azure cloud. It's designed for broad SQL Server compatibility with minimal application changes when migrating existing on-premises SQL Server databases to Azure. It combines the broad SQL Server compatibility with the benefits of a fully managed PaaS service. It's often considered a "hybrid" because it offers a blend of PaaS management with more control over instance-level features, similar to an on-premises SQL Server.
  - **SQL Server on Azure Virtual Machines (IaaS - Infrastructure as a Service)**: This model allows users to install SQL Server onto a Windows or Linux Virtual Machine (VM) hosted in Azure. Users have full control over the operating system, SQL Server installation, and configurations, just as they would with an on-premises server. This option provides the most flexibility but also requires the most management effort from the user.

- **Linux & Docker**: In a significant shift, Microsoft made SQL Server available on Linux, extending its reach to environments traditionally dominated by open-source databases.

  - **Cross-platform support via Docker images and Ubuntu installations**: SQL Server can be installed directly on various Linux distributions, including Ubuntu, Red Hat Enterprise Linux (RHEL), and SUSE Linux Enterprise Server (SLES). More importantly for modern deployment, SQL Server is also available as a **Docker image**. This allows developers and administrators to quickly deploy SQL Server in containers, enabling consistent environments, easier scaling, and integration with container orchestration platforms like Kubernetes. This cross-platform support significantly broadens SQL Server's applicability.

- **Cloud Strategy**: Deploying SQL Server in the cloud (like Azure) is driven by several key strategic advantages:
  - **Scalability**: Cloud models allow for easy scaling of database resources (CPU, memory, storage) up or down based on demand, without significant hardware investment or downtime. This is particularly beneficial for applications with fluctuating workloads.
  - **High Availability (HA)**: Cloud providers offer built-in high availability features, ensuring that the database remains operational even in the event of hardware or software failures. This often involves automatic failover, redundant storage, and geographic replication.
  - **Global Replication**: Cloud services enable easy replication of databases across different geographical regions. This improves disaster recovery capabilities, reduces latency for globally distributed users, and allows for read-scale scenarios where read operations can be directed to a nearby replica.

### 10. **SQL Server vs Other Platforms**

This section provides a comparative overview of SQL Server against other popular database platforms, highlighting key differences across various features.

| Feature       | SQL Server    | Oracle      | MySQL       | PostgreSQL    |
| ------------- | ------------- | ----------- | ----------- | ------------- |
| Platform      | Windows/Linux | Cross       | Cross       | Cross         |
| Licensing     | Proprietary   | Proprietary | Open Source | Open Source   |
| ACID Support  | Full          | Full        | Full        | Full          |
| Cloud Options | Azure         | OCI         | AWS RDS     | AWS/GCP/Azure |
| Extensions    | Limited       | PL/SQL      | Plugins     | Extensions    |

Here's an explanation of each feature in the context of the comparison:

- **Platform**: This refers to the operating systems on which the database management system can run.

  - **SQL Server**: Traditionally Windows-centric, but now also fully supports Linux.
  - **Oracle**: Known for its robust cross-platform support, running on various Unix-like systems, Linux, and Windows.
  - **MySQL**: A highly portable database, available on virtually all major operating systems including Linux, Windows, macOS, and many Unix variants.
  - **PostgreSQL**: Also known for its broad cross-platform compatibility, running on Linux, Windows, macOS, and various Unix-like systems.

- **Licensing**: This indicates how the software is typically acquired and used.

  - **SQL Server**: Primarily uses a **Proprietary** licensing model (e.g., Core-based or Server + CAL), meaning you generally need to purchase licenses from Microsoft.
  - **Oracle**: Also uses a **Proprietary** licensing model, which can be complex and expensive, especially for large-scale enterprise deployments.
  - **MySQL**: Is primarily **Open Source** (under the GNU General Public License), with a community edition that is free to use. Oracle also offers commercial enterprise versions with additional features and support.
  - **PostgreSQL**: Is purely **Open Source** (under the PostgreSQL License), making it completely free to use, modify, and distribute.

- **ACID Support**: Refers to the database system's ability to ensure **A**tomicity, **C**onsistency, **I**solation, and **D**urability of transactions. These properties are critical for data integrity, especially in concurrent environments.

  - All listed databases (SQL Server, Oracle, MySQL, PostgreSQL) offer **Full** ACID support, which is a fundamental requirement for most transactional applications.

- **Cloud Options**: Indicates the primary cloud platforms where these databases are offered as managed services.

  - **SQL Server**: Naturally has deep integration and fully managed services within **Azure** (Azure SQL Database, Azure SQL Managed Instance, SQL Server on Azure VMs).
  - **Oracle**: Has its own cloud platform, **Oracle Cloud Infrastructure (OCI)**, which offers highly optimized managed Oracle Database services. It's also available on other clouds through various arrangements.
  - **MySQL**: Widely available as a managed service on **AWS RDS** (Amazon Relational Database Service), Azure Database for MySQL, Google Cloud SQL for MySQL, and many other cloud providers.
  - **PostgreSQL**: Very popular across all major cloud providers, including **AWS (RDS and Aurora)**, **GCP (Cloud SQL)**, and **Azure (Azure Database for PostgreSQL)**, often with highly optimized and managed offerings.

- **Extensions**: Refers to the mechanisms available to extend the database's functionality beyond standard SQL, often for procedural programming or specialized features.
  - **SQL Server**: Uses **Transact-SQL (T-SQL)** for its procedural language within the database. It has various built-in functions and capabilities but might be considered "Limited" in terms of truly open-ended, community-driven external extensions compared to PostgreSQL.
  - **Oracle**: Uses **PL/SQL** (Procedural Language/SQL) as its powerful, proprietary procedural extension, widely used for stored procedures, functions, and triggers.
  - **MySQL**: Supports **Plugins** for extending functionality (e.g., for full-text search, storage engines) and standard SQL procedural language features.
  - **PostgreSQL**: Is renowned for its robust **Extensions** framework, allowing users to add powerful new functionalities (e.g., PostGIS for geospatial data, many different procedural languages like PL/pgSQL, PL/Python, PL/R). This extensibility is a major strength.

## Lab Exercises / Hands-On Practice

| #   | Task                                   | Tool    | Outcome                |
| --- | -------------------------------------- | ------- | ---------------------- |
| 1   | Install SQL Server (Developer Edition) | SSMS    | Local DB setup         |
| 2   | Explore System Databases               | SSMS    | View internal DB roles |
| 3   | Create ERD for E-Commerce DB           | mermaid | Logical data model     |
| 4   | Draw Level-1 DFD for LMS               | mermaid | Visual data flow model |

## Assessments

### Knowledge Checks (MCQs)

1.  **What is the purpose of the `tempdb` system database?**

    - A) Stores system-wide configurations
    - B) Acts as a template for new databases
    - C) Holds temporary objects and intermediate results
    - D) Manages SQL Agent jobs
    - **Answer:** C) Holds temporary objects and intermediate results
    - **Explanation:** `tempdb` is a global resource for temporary user objects (e.g., temp tables), internal objects (e.g., worktables for sorting), and version stores. It’s recreated every time SQL Server restarts.

2.  **Which of the following is NOT a relational database?**

    - A) SQL Server
    - B) MongoDB
    - C) PostgreSQL
    - D) MySQL
    - **Answer:** B) MongoDB
    - **Explanation:** MongoDB is a **NoSQL** (non-relational) document database, while the others are relational databases (RDBMS) that use tables and SQL.

3.  **Define a composite attribute in ER modeling.**

    - A) An attribute derived from other attributes
    - B) An attribute that cannot be divided into smaller parts
    - C) An attribute made up of multiple simple attributes
    - D) An attribute with a null value
    - **Answer:** C) An attribute made up of multiple simple attributes
    - **Explanation:** A **composite attribute** (e.g., `Address`) can be split into smaller sub-attributes (e.g., `Street`, `City`, `ZipCode`), unlike atomic (simple) attributes like `Age`.

4.  **Which normal form ensures no transitive dependencies?**

    - A) 1NF
    - B) 2NF
    - C) 3NF
    - D) BCNF
    - **Answer:** C) 3NF
    - **Explanation:** 3NF specifically addresses transitive dependencies, where a non-key attribute is dependent on another non-key attribute, which in turn is dependent on the primary key.

5.  **What does the `Crow’s Foot` notation `||--o{` represent?**

    - A) One-to-one mandatory
    - B) One-to-many optional
    - C) Many-to-many
    - D) One-to-one optional
    - **Answer:** B) One-to-many optional (1:N, where "many" side is optional).
    - **Explanation:** The `||` on the left signifies "exactly one" (mandatory one), while the `o{` on the right signifies "zero or many" (optional many).

6.  **Which SQL Server tool is best for automating backups?**

    - A) Azure Data Studio
    - B) SQL Server Agent
    - C) `bcp`
    - D) SSRS
    - **Answer:** B) SQL Server Agent
    - **Explanation:** SQL Server Agent is a dedicated service within SQL Server designed for scheduling and executing administrative tasks, including database backups.

7.  **What is the role of the `Query Optimizer`?**

    - A) Parses SQL syntax
    - B) Chooses the most efficient execution plan
    - C) Manages memory allocation
    - D) Enforces constraints
    - **Answer:** B) Chooses the most efficient execution plan.
    - **Explanation:** The Query Optimizer's primary function is to analyze the various ways a query can be executed and select the most cost-effective and efficient execution plan.

8.  **Which ACID property ensures committed transactions survive crashes?**

    - A) Atomicity
    - B) Consistency
    - C) Isolation
    - D) Durability
    - **Answer:** D) Durability
    - **Explanation:** Durability guarantees that once a transaction is committed, its changes are permanently stored and will survive system failures.

9.  **In a DFD, what symbol represents a data store?**

    - A) Circle
    - B) Rectangle
    - C) Parallel lines
    - D) Arrow
    - **Answer:** C) Parallel lines (or open rectangle).
    - **Explanation:** Data stores in DFDs are typically represented by two parallel lines or an open-ended rectangle, indicating a place where data is held.

10. **Which SQL Server edition is free for production use but has resource limits?**
    - A) Enterprise
    - B) Standard
    - C) Developer
    - D) Express
    - **Answer:** D) Express (free, but limited to 10GB database size).
    - **Explanation:** SQL Server Express is the free-to-use edition that can be deployed in production environments but comes with strict resource limitations, including database size.

### Short Answer Questions

**Database Fundamentals**

1.  What are the three main types of databases mentioned in the module, and what distinguishes them from each other?
2.  Explain the difference between entities and attributes in database design.
3.  What are the four ACID properties of database transactions, and why is each important?

**ER Diagrams**

4.  How would you identify a weak entity in an ER diagram?
5.  What is the difference between a simple attribute and a composite attribute? Give examples.
6.  How would you represent a many-to-many relationship in an ER diagram using Crow's Foot notation?

**Data Flow Diagrams**

7.  What are the four main components of a Data Flow Diagram (DFD)?
8.  How does a Level-1 DFD differ from a Context Diagram (Level-0 DFD)?
9.  In a shopping cart system DFD, what would be an example of a data flow between a process and a data store?

**Database Normalization**

10. What is the main goal of database normalization?
11. Describe a situation where you would need to apply 2NF to a database table.
12. Explain the concept of denormalization and when might it be appropriate to use.
13. What is the primary rule for a table to be in **1NF (First Normal Form)**?
14. How does **BCNF (Boyce-Codd Normal Form)** differ from 3NF, and when is it necessary to achieve BCNF?
15. What problem does **4NF (Fourth Normal Form)** address, and how does it relate to multi-valued dependencies?
16. Briefly explain **5NF (Fifth Normal Form)** and the type of redundancy it aims to eliminate.

**SQL Server Architecture**

17. What are the two main components of the SQL Server Database Engine?
18. Explain the role of the Buffer Pool in SQL Server.
19. What happens during the optimization phase of SQL query processing?

**SQL Server Tools and Editions**

20. What are the key differences between SQL Server Standard and Enterprise editions?
21. When would you choose to use Azure Data Studio instead of SSMS?
22. What is the purpose of the `sqlcmd` utility?

**Cloud and Cross-Platform**

23. What are the three Azure SQL deployment models mentioned in the module?
24. What advantages does running SQL Server in a Docker container provide?

### Practical Task

- Design an ERD and a Level-1 DFD for a hospital management system.
