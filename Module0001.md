# Module 1: **Introduction to Databases & SQL Server**

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

* **Books:**

  * "Database System Concepts" by Silberschatz, Korth, and Sudarshan
  * "Microsoft SQL Server 2022: A Beginner’s Guide" by Dusan Petkovic
* **Online Resources:**

  * Microsoft Learn: SQL Server Introduction
  * ERDPlus ([https://erdplus.com](https://erdplus.com))
  * Lucidchart ([https://www.lucidchart.com](https://www.lucidchart.com))
  * draw\.io ([https://app.diagrams.net](https://app.diagrams.net))
  * Azure SQL Documentation ([https://learn.microsoft.com/en-us/azure/azure-sql/](https://learn.microsoft.com/en-us/azure/azure-sql/))


## Key Concepts & Detailed Content


### 1. **Introduction to Databases**

* **Definition**: A database is a collection of interrelated data and programs designed to access and modify that data.
* **Types**:

  * Relational (SQL Server, MySQL, PostgreSQL)
  * NoSQL (MongoDB, Cassandra)
  * Hierarchical (IMS)
* **Core Elements**:

  * **Entities**: An entity is a "thing" or "object" in the real world that is distinct from all other objects. Entities can be concrete, such as a person or a book, or abstract, like a course offering or a flight reservation. An entity set is a collection of entities of the same type that share similar properties or attributes. For example, "instructor" could be an entity set representing all instructors at a university.
  * **Attributes**: Attributes are descriptive properties that each member of an entity set possesses. They define the characteristics of an entity. For instance, an instructor entity set might have attributes like ID, name, dept_name, and salary. Each entity will have its own specific value for each attribute.
  * **Relationships**: Relationships describe how entities are connected or associated with each other. For example, in a university database, a student entity might "take" a course offering entity, and an instructor entity "teaches" a course offering. These "takes" and "teaches" are examples of relationships between entities. The Entity-Relationship (E-R) model helps in mapping these real-world meanings and interactions into a conceptual schema for the database.

### 2. **Database Design Using ER Diagrams**

* **Entity Types**:

  * Strong (independent), Weak (dependent)
* **Attributes**:

  * Simple, Composite, Derived
* **Relationships**:

  * One-to-One (1:1), One-to-Many (1\:N), Many-to-Many (M\:N)
* **Notation and Symbols**: Crow’s foot, UML, etc.
* **Tools**: draw\.io, Lucidchart, ERDPlus


### 3. **System Analysis Using Data Flow Diagrams (DFDs)**

* **DFD Levels**:

  * Context (Level 0), Level-1 (Detailed)
* **Elements**:

  * Processes, Data Stores, Data Flows, External Entities
* **Mapping**:

  * Convert DFD elements into relational schema components
* **Case Study**:

  * Design ERD + DFD for Library Management System


### 4. **Data Structures & Normalization**

* **Table Anatomy**:

  * Rows, Columns, Data Types
* **Constraints**:

  * `NOT NULL`, `UNIQUE`, `DEFAULT`, `CHECK`, `PRIMARY KEY`, `FOREIGN KEY`
* **Normalization**:

  * 1NF: Atomicity
  * 2NF: Partial Dependency Removal
  * 3NF: Transitive Dependency Removal
* **Denormalization**:

  * Performance optimization strategy


### 5. **Overview of SQL Server**

* **What is RDBMS?**

  * Relational model by Codd
* **SQL Server Editions**:

  * Enterprise, Standard, Developer, Express
* **Use Cases**:

  * OLTP, Data Warehousing, Business Intelligence
* **Licensing Models**:

  * Core-based, CAL-based


### 6. **SQL Server Architecture**

* **Components**:

  * **Database Engine**: Core service
  * **Storage Engine**: Physical data management
  * **Query Processor**: Execution of T-SQL
  * **Buffer Pool**: Memory management
  * **Lock Manager**: Concurrency control
* **Query Lifecycle**:

  * Parsing → Optimization → Execution


### 7. **System Databases in SQL Server**

| Database   | Description                |
| ---------- | -------------------------- |
| `master`   | System-wide configurations |
| `model`    | Template for new databases |
| `msdb`     | SQL Agent, scheduling      |
| `tempdb`   | Temporary objects          |
| `resource` | Internal system metadata   |


### 8. **SQL Server Tool Ecosystem**

* **GUI Tools**:

  * SSMS (SQL Server Management Studio)
  * Azure Data Studio
* **CLI Tools**:

  * `sqlcmd`, `bcp`, PowerShell
* **Development Integration**:

  * Visual Studio + SSDT
  * Git integration, CI/CD support


### 9. **SQL Server on Cloud & Linux**

* **Azure SQL Models**:

  * Azure SQL DB (PaaS), Managed Instance (hybrid), SQL VM (IaaS)
* **Linux & Docker**:

  * Cross-platform support via Docker images and Ubuntu installations
* **Cloud Strategy**:

  * Scalability, HA, global replication


### 10. **SQL Server vs Other Platforms**

| Feature       | SQL Server    | Oracle      | MySQL       | PostgreSQL    |
| ------------- | ------------- | ----------- | ----------- | ------------- |
| Platform      | Windows/Linux | Cross       | Cross       | Cross         |
| Licensing     | Proprietary   | Proprietary | Open Source | Open Source   |
| ACID Support  | Full          | Full        | Full        | Full          |
| Cloud Options | Azure         | OCI         | AWS RDS     | AWS/GCP/Azure |
| Extensions    | Limited       | PL/SQL      | Plugins     | Extensions    |

* **NoSQL Brief**:

  * MongoDB: Document-based
  * Cassandra: Wide-column for distributed setups


## 🧪 Lab Exercises / Hands-On Practice

| # | Task                                             | Tool                | Outcome                |
| - | ------------------------------------------------ | ------------------- | ---------------------- |
| 1 | Install SQL Server (Developer Edition or Docker) | SSMS/Docker         | Local DB setup         |
| 2 | Explore System Databases                         | SSMS                | View internal DB roles |
| 3 | Create ERD for E-Commerce DB                     | draw\.io/Lucidchart | Logical data model     |
| 4 | Draw Level-1 DFD for LMS                         | draw\.io            | Visual data flow model |
| 5 | Compare schema in SQL Server vs PostgreSQL       | SSMS + pgAdmin      | Feature comparison     |


## Assessments

### Knowledge Checks (MCQs)

1. What is the purpose of the `tempdb` system database?
2. Which of the following is NOT a relational database?
3. Define a composite attribute in ER modeling.

### Short Answer Questions

* Explain 2NF with an example.
* Compare SQL Server and MySQL for web applications.

### Practical Task

* Design an ERD and a Level-1 DFD for a hospital management system.
* Submission via GitHub repository.


## Deliverables Checklist

| Deliverable        | Format       | Submission Mode |
| ------------------ | ------------ | --------------- |
| ERD Design         | .png/.drawio | GitHub          |
| DFD Diagram        | .pdf/.png    | GitHub          |
| Lab Queries        | .sql scripts | GitHub          |
| Assessment Answers | .docx/.md    | GitHub          |
