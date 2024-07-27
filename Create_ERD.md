# Create Database Coffee
Create database Coffee

# Identify datatype
## staff table/entities
(staff_id int primary key not null,  
first_name varchar(120) not null,   
last_name varchar(120) not null,  
position varchar(120),  
start_date date not null,  
location varchar(35))
## sales_outlet
(sale_outlet_id int primary key not null,  
sale_outlet_type varchar(120) not null,   
address varchar(500) not null,  
city varchar(120),  
telephone varchar(25) not null,  
postal_code varchar(5),  
manager int)
## customer
(customer_id int primary key not null,  
customer_name varchar(120) not null,   
customer_email varchar(120) not null,  
customer_since date not null,    
customer_card_numer varchar(25),  
birthday date,  
gender char(1))
## product
(product_id int primary key,     
product_category varchar(30),    
product_type varchar(120),  
product_name varchar(150),   
description varchar(1000),   
price decimal(5,2)  
## sale_transactions
(transaction_id int primary key not null,  
date date not null,   
time time not null,   
sales_outlet int not null,   
staff_id int not null,   
product_id int not null,   
quantity int,   
price decimal(5,2))  

# Create ERD 
  - Normalize tables
    - Review the data in the sales transaction table. Note that the transaction id column does not contain unique values because some transactions include multiple products.
      - Determine which product_id should be stored in a separate table to remove the repeating rows and to put this table into second normal form (2NF).
drop table if exists sales_transaction;
      - Add a new table named sales_detail to the ERD, define the columns in the new table, and delete the moved columns from the sales transaction table, leaving a matching column in each of the two tables to create a relationship between them later.

        - sales_detail table  
            (transaction_id int,
              product_id int not null,   
              quantity int,   
              price decimal(5,2))
        - (transaction_id int primary key not null,  
              date date not null,   
              time time not null,   
              sales_outlet int not null,   
              staff_id int not null)
  -  Review the data in the product table. Note that the product category and product type columns contain redundant data.  
      -  Determine which product_type and product_category should be stored in a separate table to reduce redundant data and to put this table into a second normal form.  
      - Add a new table named product_type, product_category to the ERD, define the columns in the new table, and delete the moved columns from the product table, leaving a matching column in each of the two tables to create a relationship between them later.      
           - table Product  
              (product_id int primary key,       
              product_category_id int,      
              product_type_id  int,    
              product_name varchar(150),     
              description varchar(1000),     
              price decimal(5,2)   
          - table product_type  
             (product_type_id  int primary key not null,    
              product_type varchar(120))  
          - table product_category   
             (product_category_id int primary key not null,    
              product_category varchar(120));      
  - Define key and relationship
DROP TABLE IF EXISTS public.staff CASCADE;
DROP TABLE IF EXISTS public.sales_outlet CASCADE;
DROP TABLE IF EXISTS public.customer CASCADE;
DROP TABLE IF EXISTS public.sales_detail CASCADE;
DROP TABLE IF EXISTS public.product CASCADE;
DROP TABLE IF EXISTS public.product_type CASCADE;
DROP TABLE IF EXISTS public.sales_transaction CASCADE;
--

CREATE TABLE public.staff
(
    staff_id integer,
    first_name character varying(50),
    last_name character varying(50),
    "position" character varying(50),
    start_date date,
    location character varying(5),
    PRIMARY KEY (staff_id)
);

CREATE TABLE public.sales_outlet
(
    sales_outlet_id integer,
    sales_outlet_type character varying(20),
    address character varying(50),
    city character varying(40),
    telephone character varying(15),
    postal_code integer,
    manager integer,
    PRIMARY KEY (sales_outlet_id)
);

CREATE TABLE public.customer
(
    customer_id integer,
    customer_name character varying(50),
    email character varying(50),
    reg_date date,
    card_number character varying(15),
    date_of_birth date,
    gender character(1),
    PRIMARY KEY (customer_id)
);

CREATE TABLE public.sales_detail
(
    sales_detail_id integer,
    transaction_id integer,
    product_id integer,
    quantity integer,
    price double precision,
    PRIMARY KEY (sales_detail_id)
);


CREATE TABLE public.product
(
    product_id integer,
    product_name character varying(100),
    description character varying(250),
    product_price double precision,
    product_type_id integer,
    PRIMARY KEY (product_id)
);

CREATE TABLE public.product_type
(
    product_type_id integer,
    product_type character varying(50),
    product_category character varying(50),
    PRIMARY KEY (product_type_id)
);

CREATE TABLE public.sales_transaction
(
    transaction_id integer,
    transaction_date date,
    transaction_time time without time zone,
    sales_outlet_id integer,
    staff_id integer,
    customer_id integer,
    PRIMARY KEY (transaction_id)
);

ALTER TABLE public.sales_transaction
    ADD FOREIGN KEY (staff_id)
    REFERENCES public.staff (staff_id)
    NOT VALID;


ALTER TABLE public.sales_transaction
    ADD FOREIGN KEY (sales_outlet_id)
    REFERENCES public.sales_outlet (sales_outlet_id)
    NOT VALID;


ALTER TABLE public.sales_transaction
    ADD FOREIGN KEY (customer_id)
    REFERENCES public.customer (customer_id)
    NOT VALID;


ALTER TABLE public.sales_detail
    ADD FOREIGN KEY (transaction_id)
    REFERENCES public.sales_transaction (transaction_id)
    NOT VALID;


ALTER TABLE public.sales_detail
    ADD FOREIGN KEY (product_id)
    REFERENCES public.product (product_id)
    NOT VALID;


ALTER TABLE public.product
    ADD FOREIGN KEY (product_type_id)
    REFERENCES public.product_type (product_type_id)
    NOT VALID;

END;
