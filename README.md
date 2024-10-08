# Library-Management--SQL-Project
---

## Table Of Content
---
1. [Project Overview](#project-overview)
2. [Objectives](#objectives)
3. [Project Structure](#project-structure) 
4.

## Project Overview
---
This project demonstrates the implementation of a Library Management System using SQL. It includes creating and managing tables, performing CRUD operations, and executing advanced SQL queries. The goal is to showcase skills in database design, manipulation, and querying.

## Objectives
---
1. **Set up the Library Management System Database**: Create and populate the database with tables for branches, employees, members, books, issued status, and return status.
2. **CRUD Operations**: Perform Create, Read, Update, and Delete operations on the data.
3. **CTAS (Create Table As Select)**: Utilize CTAS to create new tables based on query results.
4. **Advanced SQL Queries**: Develop complex queries to analyze and retrieve specific data.

## Project Structure
---
### 1. Database Setup
- **Database Creation**: Created a database named `library_db`.
- **Table Creation**: Created tables for branches, employees, members, books, issued status, and return status. Each table includes relevant columns and relationships.

```sql

create database library_project1;
use library_project1;
CREATE TABLE branch
(
            branch_id VARCHAR(10) PRIMARY KEY,
            manager_id VARCHAR(10),
            branch_address VARCHAR(30),
            contact_no VARCHAR(15)
);


-- Create table "Employee"
DROP TABLE IF EXISTS employees;
CREATE TABLE employees
(
            emp_id VARCHAR(10) PRIMARY KEY,
            emp_name VARCHAR(30),
            position VARCHAR(30),
            salary DECIMAL(10,2),
            branch_id VARCHAR(10),
            FOREIGN KEY (branch_id) REFERENCES  branch(branch_id)
);


-- Create table "Members"
DROP TABLE IF EXISTS members;
CREATE TABLE members
(
            member_id VARCHAR(10) PRIMARY KEY,
            member_name VARCHAR(30),
            member_address VARCHAR(30),
            reg_date DATE
);



-- Create table "Books"
DROP TABLE IF EXISTS books;
CREATE TABLE books
(
            isbn VARCHAR(50) PRIMARY KEY,
            book_title VARCHAR(80),
            category VARCHAR(30),
            rental_price DECIMAL(10,2),
            status VARCHAR(10),
            author VARCHAR(30),
            publisher VARCHAR(30)
);

-- Create table "IssueStatus"
DROP TABLE IF EXISTS issued_status;
CREATE TABLE issued_status
(
            issued_id VARCHAR(10) PRIMARY KEY,
            issued_member_id VARCHAR(30),
            issued_book_name VARCHAR(80),
            issued_date DATE,
            issued_book_isbn VARCHAR(50),
            issued_emp_id VARCHAR(10),
            FOREIGN KEY (issued_member_id) REFERENCES members(member_id),
            FOREIGN KEY (issued_emp_id) REFERENCES employees(emp_id),
            FOREIGN KEY (issued_book_isbn) REFERENCES books(isbn) 
);

-- Create table "ReturnStatus"
DROP TABLE IF EXISTS return_status;
CREATE TABLE return_status
(
            return_id VARCHAR(10) PRIMARY KEY,
            issued_id VARCHAR(30),
            return_book_name VARCHAR(80),
            return_date DATE,
            return_book_isbn VARCHAR(50),
            FOREIGN KEY (return_book_isbn) REFERENCES books(isbn)
);
```
**Q1. Create a New Book Record, ('978-1-60129-456-2', 'To Kill a Mockingbird', 'Classic', 6.00, 'yes', 'Harper Lee', 'J.B. Lippincott & Co.')**
```sql
insert into books(isbn, book_title, category, rental_price, status, author, publishers)
values('978-1-60129-456-2', 'To Kill a Mockingbird', 'Classic', 6.00, 'yes', 'Harper Lee', 'J.B. Lippincott & Co.');
```

**Q2. Update an Existing Member's Address.**

```sql
update  members
Set member_address= '456 Main town' where member_id = 'C103';
```

**Q3.Delete a Record from the Issued Status Table**
-- Objective: Delete the record with issued_id = 'IS121' from the issued_status table.

```sql
Delete from Issued_Status
where issued_id='IS121'
```
**Q4: Retrieve All Books Issued by a Specific Employee**
-- Objective: Select all books issued by the employee with emp_id = 'E101'.
```sql
SELECT * FROM issued_status
WHERE issued_emp_id = 'E101'
```
**Q5: List Members Who Have Issued More Than One Book**
-- Objective: Use GROUP BY to find members who have issued more than one book.
```sql
Select issued_emp_id , count(issued_id) as count_of_issued_books 
from issued_status
group by issued_emp_id
having count(issued_id)> 1;
```
**Q6: Create Summary Tables: Used CTAS to generate new tables based on query results - each book and total book_issued_cnt.**
```sql
create table book_count as 
select b.book_title, i.issued_book_isbn , count(i.issued_id) as total_book_issued_cnt   
from issued_status as i 
join books as b 
on b.isbn= i.issued_book_isbn 
group by i.issued_book_isbn,b.book_title ;

select * from book_count;
```
### 4. Data Analysis & Findings

The following SQL queries were used to address specific questions:

**Q7. Retrieve All Books in a Specific Category.**
```sql
select book_title, category from books
order by category;            -- This will give for each category , To find for specific category , use where clause .

select book_title, category from books
where category='Children';
```
**Q8. Find Total Rental Income by Category.** Objectives- Calculate for each book and total price for books should be there 
```sql
Select b.category,sum(b.rental_price) as total_rental_income 
from books as b 
join issued_status as i  
on b.isbn= i.issued_book_isbn 
group by b.category
```
**Q9. List Members Who Registered in the Last 180 Days**:
```sql
select * from members 
where reg_date>=current_date - interval 180 day;
```

**Q10.List Employees with Their Branch Manager's Name and their branch details.**





















