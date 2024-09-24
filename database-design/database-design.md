# Database Design

## Introduction
In this lab you will execute some SQL operations, practice using JSON and use MySQL Document Store.

Estimated Lab Time: 20 minutes

### Objectives
In this lab, you will:
* Work with SQL and JSON
* Understand the functioning of MySQL Document Store 
* Practice some CRUD operations.

> **Note:** 
  * Server: mysql1

## Task 1: Work with SQL
1. Connect to your <span style="color:red">mysql-advanced</span> with admin user
    ```
    <span style="color:green">shell></span> <copy>mysqlsh -uadmin -p -h mysql1 -P 3307 --sql</copy>
    ```

2. Create a new table poi
     ```
    <span style="color:blue">mysql></span> <copy>use world;</copy>
    ```
     ```
    <span style="color:blue">mysql></span> <copy>CREATE TABLE if not exists poi (x Int, y INT, z INT);</copy>
    ```

3. Add to the table a new column for id used for large integer values
     ```
    <span style="color:blue">mysql></span> <copy>alter table poi add id bigint;</copy>
    ```
    ```
    <span style="color:blue">mysql></span> <copy>ALTER TABLE poi ADD PRIMARY KEY (id);</copy>
    ```

4. Create a copy of your city table
    ```
    <span style="color:blue">mysql></span> <copy>create table city_part as select * from city;</copy>
    ```

5. How many records does it contain?
    ```
    <span style="color:blue">mysql></span> <copy>SELECT count(*) FROM city_part;</copy>
    ```

6. How many records city table contain?
    ```
    <span style="color:blue">mysql></span> <copy>SELECT count(*) FROM city;</copy>
    ```

7. Verify the difference of the two table creation (there is a big one!)
    ```
    <span style="color:blue">mysql></span> <copy>show create table city\G</copy>
    ```
    ```
    <span style="color:blue">mysql></span> <copy>show create table city_part\G</copy>
    ```

8. Create an index on new table
    ```
    <span style="color:blue">mysql></span> <copy>CREATE INDEX myidindex ON city_part (ID);</copy>
    ```

9. Check table statistics. What is the Cardinality (=unique records) of primary key?
    ```
    <span style="color:blue">mysql></span> <copy>SELECT * FROM INFORMATION_SCHEMA.STATISTICS WHERE table_name ='city' and table_schema='world'\G</copy>
    ```

10. Delete some columns (Population and CountryCode)
    ```
    <span style="color:blue">mysql></span> <copy>ALTER TABLE city_part DROP COLUMN Population;</copy>
    ```
    ```
    <span style="color:blue">mysql></span> <copy>ALTER TABLE city_part DROP COLUMN CountryCode;</copy>
    ```

11. Optimize the table
    ```
    <span style="color:blue">mysql></span> <copy>OPTIMIZE TABLE city_part;</copy>
    ```
    > **Note:** warning is expected: https://dev.mysql.com/doc/refman/8.0/en/optimize-table.html

12. Update table statistics
    ```
    <span style="color:blue">mysql></span> <copy>ANALYZE TABLE city_part;</copy>
    ```

13. Exit from client
    ```
    <span style="color:blue">mysql></span> <copy>\q</copy>
    ```


## Acknowledgements
* **Author** - Marco Carlessi, Principal Sales Consultant
* **Contributors** -  Perside Foster, MySQL Solution Engineering, Selena Sánchez, MySQL Solutions Engineer
* **Last Updated By/Date** - Selena Sánchez, MySQL Solution Engineering, May 2023
