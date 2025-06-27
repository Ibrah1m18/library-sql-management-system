# 📚 Library Management System with SQL

## 📌 Table of Contents

- [Project Overview](#project-overview)
- [Objectives](#objectives)
- [Project Structure](#project-structure)
- [CRUD Operations](#crud-operations)
- [CTAS Examples](#ctas-examples)
- [Advanced SQL Tasks](#advanced-sql-tasks)
- [Reports](#reports)
- [Conclusion](#conclusion)
- [How to Use](#how-to-use)
- [Author](#author---zero-analyst)

---

## 📄 Project Overview

**Project Title**: Library Management System\
**Level**: Intermediate\
**Database**: `library_db`

This project demonstrates the implementation of a Library Management System using SQL. It includes creating and managing tables, performing CRUD operations, and executing advanced SQL queries. The goal is to showcase skills in database design, manipulation, and querying.



---

## 📊 Objectives

1. **Database Setup**: Create and populate tables (branches, employees, members, books, issue and return status).
2. **CRUD Operations**: Perform insert, read, update, and delete actions.
3. **CTAS Usage**: Use CTAS to generate summary tables.
4. **Data Analysis**: Write advanced queries to extract insights.

---

## 🗂️ Project Structure

### Database Setup



Tables created include:

- `branch`
- `employees`
- `members`
- `books`
- `issued_status`
- `return_status`

> Full DDL scripts included in `scripts/database_setup.sql`

---

## 📋 CRUD Operations

### ✅ Task 1: Create a New Book Record

```sql
INSERT INTO books(isbn, book_title, category, rental_price, status, author, publisher)
VALUES('978-1-60129-456-2', 'To Kill a Mockingbird', 'Classic', 6.00, 'yes', 'Harper Lee', 'J.B. Lippincott & Co.');
```

### ✅ Task 2: Update Member Address

```sql
UPDATE members
SET member_address = '125 Oak St'
WHERE member_id = 'C103';
```

### ✅ Task 3: Delete Issued Record

```sql
DELETE FROM issued_status
WHERE issued_id = 'IS121';
```

### ✅ Task 4: Books Issued by Employee

```sql
SELECT * FROM issued_status
WHERE issued_emp_id = 'E101';
```

### ✅ Task 5: Members Issued More Than One Book

```sql
SELECT issued_member_id, COUNT(*)
FROM issued_status
GROUP BY issued_member_id
HAVING COUNT(*) > 1;
```

---

## 📑 CTAS Examples

### ✅ Task 6: Book Issue Count Table

```sql
CREATE TABLE book_issued_cnt AS
SELECT b.isbn, b.book_title, COUNT(ist.issued_id) AS issue_count
FROM issued_status ist
JOIN books b ON ist.issued_book_isbn = b.isbn
GROUP BY b.isbn, b.book_title;
```

---

## 🔢 Advanced SQL Tasks

### ✅ Task 7: Books in Category 'Classic'

```sql
SELECT * FROM books WHERE category = 'Classic';
```

### ✅ Task 8: Total Rental Income by Category

```sql
SELECT b.category, SUM(b.rental_price), COUNT(*)
FROM issued_status ist
JOIN books b ON b.isbn = ist.issued_book_isbn
GROUP BY b.category;
```

### ✅ Task 9: Members Registered in Last 180 Days

```sql
SELECT * FROM members
WHERE reg_date >= CURRENT_DATE - INTERVAL '180 days';
```

### ✅ Task 10: Employees and Their Branch Manager

```sql
SELECT e1.emp_id, e1.emp_name, e1.position, b.*, e2.emp_name AS manager
FROM employees e1
JOIN branch b ON e1.branch_id = b.branch_id
JOIN employees e2 ON e2.emp_id = b.manager_id;
```

### ✅ Task 11: Expensive Books Table

```sql
CREATE TABLE expensive_books AS
SELECT * FROM books WHERE rental_price > 7.00;
```

### ✅ Task 12: Books Not Yet Returned

```sql
SELECT * FROM issued_status ist
LEFT JOIN return_status rs ON rs.issued_id = ist.issued_id
WHERE rs.return_id IS NULL;
```

### ✅ Task 13: Members With Overdue Books

```sql
SELECT ist.issued_member_id, m.member_name, bk.book_title, ist.issued_date, CURRENT_DATE - ist.issued_date AS overdue_days
FROM issued_status ist
JOIN members m ON m.member_id = ist.issued_member_id
JOIN books bk ON bk.isbn = ist.issued_book_isbn
LEFT JOIN return_status rs ON rs.issued_id = ist.issued_id
WHERE rs.return_date IS NULL AND (CURRENT_DATE - ist.issued_date) > 30;
```

### ✅ Task 14: Procedure to Mark Book as Returned

> See `scripts/procedures.sql` for full implementation

### ✅ Task 15: Branch Performance Report

```sql
CREATE TABLE branch_reports AS
SELECT b.branch_id, b.manager_id, COUNT(ist.issued_id) AS number_book_issued,
       COUNT(rs.return_id) AS number_of_book_return,
       SUM(bk.rental_price) AS total_revenue
FROM issued_status ist
JOIN employees e ON e.emp_id = ist.issued_emp_id
JOIN branch b ON e.branch_id = b.branch_id
LEFT JOIN return_status rs ON rs.issued_id = ist.issued_id
JOIN books bk ON ist.issued_book_isbn = bk.isbn
GROUP BY b.branch_id, b.manager_id;
```

### ✅ Task 16: Active Members Table (Last 2 Months)

```sql
CREATE TABLE active_members AS
SELECT * FROM members
WHERE member_id IN (
    SELECT DISTINCT issued_member_id
    FROM issued_status
    WHERE issued_date >= CURRENT_DATE - INTERVAL '2 month'
);
```

### ✅ Task 17: Top 3 Employees by Issues Processed

```sql
SELECT e.emp_name, b.branch_id, COUNT(ist.issued_id) AS no_book_issued
FROM issued_status ist
JOIN employees e ON e.emp_id = ist.issued_emp_id
JOIN branch b ON e.branch_id = b.branch_id
GROUP BY e.emp_name, b.branch_id
ORDER BY no_book_issued DESC
LIMIT 3;
```

---

## 📊 Reports

- Database Schema with ERD
- Analysis: Issued books, branch revenue, member trends
- Aggregations: Top books, employee performance, overdue reports

---

## 🔺 Conclusion

This project demonstrates strong foundational SQL skills across schema design, data manipulation, reporting, and procedural logic. It serves as a practical mini-case study for managing relational data.

---

## Author

**Ibrahim Ahmed**  
Aspiring Data Analyst | SQL | Power BI | Python  
📫 [Connect on LinkedIn](https://www.linkedin.com/in/ibrahim-ahmed-572475143/)

