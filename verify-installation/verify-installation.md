# Import sample databases and play with the MySQL connection

## Introduction
In this lab you will verify and start using your new MySQL Installation

Estimated Lab Time: 20 minutes

### Objectives
In this lab, you will:
* Install test databases for labs (world and employees)
* Have a look on useful statements
* Remove community software

> **Note:** 
  * Server: mysql1

## Task 1: Import sample databases

1. If not already connected, connect to mysql1 server thought app-srv
    ```
    <span style="color:green">shell-app-srv$</span> <copy>ssh -i $HOME/sshkeys/id_rsa_mysql1 opc@mysql1 </copy>
    ```

2. Import the world database, that will be used later, from c:\workshop\databases\world. You can do it with mysql client
    ```
    <span style="color:green">shell-mysql1></span> <copy>mysql -uadmin -p -P3307 -h mysql1 </workshop/databases/world/world.sql</copy>
    ```

3. Import the employees demo database that is in /workshop/databases folder.
    ```
    <span style="color:green">shell-mysql1></span> <copy>cd /workshop/databases/employees</copy>
    ```
    ```
    <span style="color:green">shell-mysql1></span> <copy>mysql -uadmin -p -P3307 -h mysql1 < ./employees.sql</copy>
    ```

4. Check that we have now the world and employees demo databases in our instance

    ```
    <span style="color:green">shell-mysql1></span> <copy>mysql -uadmin -p -P3307 -h mysql1 -e "SHOW DATABASES"</copy>
    ```

## Task 2: Understand MySQL Connections

1. Connect to mysql1 server through app-srv
    ```
    <span style="color:green">shell-app-srv$</span><copy>ssh -i $HOME/sshkeys/id_rsa_mysql1 opc@mysql1</copy>
    ```
    
  Please note that now you have 2 instances on the same server: one on  3306 (community) and one on 3307 (commercial).
  MySQL (as default) interpret localhost as socket and not the 127.0.0.1 TCP address.This may end with strange behaviors and errors.


  Here we practice connecting in various way and check what is working and what is not (note: port 3310 is intentionally wrong).

2. Use the command in table below to test different connection strings and check the result. If the result is not clear to you, please ask an explanation to your instructor. Please note that “-p” lowercase refers to password, “-P” uppercase refer to the TCP port.
  Don’t be confused by the client version and check these lines, to understand “why” (not all are always available...)
  * Current user:
  * Connection:
  * UNIX socket:
  * TCP port:
  * Server version:

  | Command | Port 3306 or 3307? | SSL is used? | Socket or TCP connection? |
  | ------- | ------------------ | ------------ | ------------------------- |
  | <span style="color:green">shell></span> mysql -u root -p<br> <span style="color:blue"> mysql></span> status; |   |   |   |
  | <span style="color:green">shell></span> mysql -u root -p -P **3306** <br> <span style="color:blue"> mysql></span> status; |   |   |   |
  | <span style="color:green">shell></span> mysql -u root -p -P **3307** <br> <span style="color:blue"> mysql></span> status; |   |   |   |
  | <span style="color:green">shell></span> mysql -u root -p -h **localhost** -P **3307** <br> <span style="color:blue"> mysql></span> status; |   |   |   |
  | <span style="color:green">shell></span> mysql -u root -p -h **127.0.0.1** -P **3307** <br> <span style="color:blue"> mysql></span> status; |   |   |   |
  | <span style="color:green">shell></span> mysql -u **admin** -p -h **mysql1** -P **3307** <br> <span style="color:blue"> mysql></span> status; |   |   |   |

3. We can now exit from mysql connection and return to OS shell

    ```
    <span style="color:blue">mysql></span> <copy>\q</copy>
    ```


## Task 3: Remove Community installation

1. Now that we better understood how to connect, we can remove the community installation
 
    ```
    <span style="color:green">shell-mysql1></span> <copy>sudo yum remove mysql mysql-server</copy>
    ```


## Acknowledgements
* **Author** - Marco Carlessi, Principal Sales Consultant
* **Contributors** -  Perside Foster, MySQL Solution Engineering, Selena Sánchez, MySQL Solutions Engineer
* **Last Updated By/Date** - Selena Sánchez, MySQL Solution Engineering, May 2023
