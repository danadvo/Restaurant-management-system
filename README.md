# Restaurant management system
### Written in C++, using OOP principals
***This project was done as part of SPL course, grade: 100***

This program simulates a restaurant management system. The program will open the restaurant, assign customers to tables, make orders,
provide bills to the tables, and other requests as [described below](https://github.com/danadvo/restaurant-management-system/new/master?readme=1#-actions-that-may-be-done-by-the-user-).

The program will get a config file as an input, which includes all required information about the
restaurant opening - number of tables, number of available seats in each table, and details
about the dishes in the menu. To read more about the format of the input file [click here](https://github.com/danadvo/restaurant-management-system/new/master?readme=1#input) .

There are 4 types of customers in this restaurant, each customer type has its own way of
ordering from the menu (an ordering strategy) to read more [click here](https://github.com/danadvo/restaurant-management-system/new/master?readme=1#-types-of-customers-). An order may be taken from a table more than
once, in such cases some customers may order a different dish. 

Each table in the restaurant has a limited amount of seats available (this info is provided in the
config file). The restaurant can’t connect tables together, nor accommodates more customers
than the number of seats available in a table. In this restaurant, it’s impossible to add new
customers to an open table, but it’s possible to move customers from one table to another.
A bill of a table is the total price of all dishes ordered for that table. 



#### <ins> Running the project: </ins>
- The project will be build (compile + link) by running the following command: "make".

- The makefile should compile the cpp files into the bin/ folder and create an executable
named ”rest” and place it also in the bin/ folder.



#### <ins>Input:</ins>
The program gets a config file as an input, which includes all required information about the
restaurant opening :
- Parameter 1: number of tables in the restaurant.
- Parameter 2: a list of tables capacities (maximum seats available) separated by comma:

               <table 1 number of seats>,<table 2 number of seats> …
- Parameter 3: a list of dishes, each dish in a separate line, including dish name, dish
               type (3-letters code: Vegetarian – VEG, Beverage – BVG, Alcoholic – ALC, Spicy –
               SPC) and a price separated by comma:
               
               <dish name>,<dish type>,<dish price>
       
       
       
#### <ins> Actions that may be done by the user: </ins>
###### Implemented in class Base Action
**1. Open Table** – Opens a given table and assigns a list of customers to it.

  If the table doesn't exist or is already open, this action will result in an error: “Table does not
exist or is already open”.

<ins> Syntax: </ins> 

  open <table_id> <customer_1>,< customer_1_strategy> <customer_2>,<
  customer_2_ strategy> ….

<ins> Example: </ins>

  "open 2 Shalom,veg Dan,chp Alice,veg Bob,spc" 
  
will open table number 2, and will seat the four customers in it, in case that table number 2 was not open and it has at least 4
available seats.

**2. Order** – Takes an order from a given table. This function will perform an order from
each customer in the table, and each customer will order according to his strategy. After
finishing with the orders, a list of all orders should be printed. 

If the table doesn't exist, or isn't open, this action should result in an error: “Table does not exist or is not open”.

<ins>Syntax:</ins>

  order <table_id>

<ins>Example:</ins>

  "order 2" – Takes an order from table number 2, and then prints:
  
  Shalom ordered Salad
  
  Shalom ordered Milkshake
  
  Dan ordered Water
  
  Alice ordered Chili con carne


**3. Move customer** - Moves a customer from one table to another. Also moves all orders
made by this customer from the bill of the origin table to the bill of the destination table.
If the origin table has no customers left after this move, the program will close the origin
table. 

If either the origin or destination table are closed or doesn't exist, or if no
customer with the received id is in the origin table, or if the destination table has no
available seats for additional customers, this action should result in an error: “Cannot
move customer”.

<ins>Syntax:</ins>

move <origin_table_id> <dest_table_id> <customer_id>

<ins>Example:</ins>

"move 2 3 5" will move customer 5 from table 2 to table 3.

**4. Close** – Closes a given table. Should print the bill of the table to the screen. After this
action the table should be open for new customers. 

If the table doesn't exist, or isn't open, this action should result in an error: “Table does not exist or is not open”.

<ins>Syntax:</ins>

close <table_id>

<ins>Example:</ins>

"close 2" closes table 2, and then prints:

  Table 2 was closed. Bill 500NIS 

**5. Close all** – Closes all tables in the restaurant, and then closes the restaurant and exits.

The bills of all the tables that were closed by that action should be printed sorted by the
table id in an increasing order. 

Note that if all tables are closed in the restaurant, the action will just close the restaurant and exit. This action never results in an error.

<ins>Syntax:</ins>

closeall

<ins>Example:</ins>

"closeall" will print:

  Table 2 was closed. Bill 500NIS
  
  Table 4 was closed. Bill 200NIS
  
  Table 5 was closed. Bill 600NIS
  
**6. Print menu** – Prints the menu of the restaurant. This action never results in an error.

Each dish in the menu should be printed in the following manner:

<dish_name> <dish_type> <dish_price>

<ins>Syntax:</ins>

menu

<ins>Example:</ins>

"menu" will print:

Salad VEG 40NIS

Water BVG 20NIS

Chili con carne SPC 400NIS

**7. Print table status** - Prints a status report of a given table. The report should include
the table status, a list of customers that are seating in the table, and a list of orders
done by each customer. 

If the table is closed, only the table status should be printed.
This action never results in an error.

<ins>Syntax:</ins>

status <table_id>

<ins>Example:</ins>

 In table 2 (table id = 2), where Shalom ordered salad and water, Dan
ordered salad, and Alice ordered Chili con carne, "status 2" will print:

Table 2 status: open

Customers:

0 Shalom

1 Dan

2 Alice

Orders:

Salad 40NIS 0

Water 20NIS 0

Salad 40NIS 1

Chili con carne 400NIS 2

Current Bill: 500NIS

**7. Print actions log** – Prints all the actions that were performed by the user (excluding
current log action), from the first action to the last action. 

This action never results in an error.

<ins>Syntax:</ins>

log

<ins>Example:</ins>

In case these are the actions that were performed since the restaurant was opened:

open 2 Jon,spc Pete,veg

open 2 Roger,spc Keith,alc

Error: Table does not exist or is already open

Then the “log” action will print:

open 2 Jon,spc Pete,veg Completed

open 2 Roger,spc Keith,alc Error: Table does not exist or is already open

**8. Backup restaurant** – save all restaurant information (restaurant’s status, tables, orders,
menu and actions history) in a global variable called “backup”. The program can keep
only one backup: If it's called multiple times, the latest restaurant's status will be stored
and overwrite the previous one. This action never results in an error.

<ins>Syntax:</ins>

backup

**9. Restore restaurant** – restore the backed up restaurant status and overwrite the current
restaurant status (including restaurant’s status, tables, orders, menu and actions
history). 

If this action is called before backup action is called (which means “backup” is
empty), then this action should result in an error: “No backup available”.

<ins>Syntax:</ins>

restore



#### <ins> Types of customers: </ins>
###### Implemented in class Customer

**1. Vegetarian Customer** – This is a customer that always orders the vegetarian dish with the
smallest id in the menu, and the most expensive beverage (Non-alcoholic). (3-letter code –
veg)

**2. Cheap Customer** – This is a customer that always orders the cheapest dish in the menu. This
customer orders only once. (3-letter code – chp)

**3. Spicy Customer** – This is a customer that orders the most expensive spicy dish in the menu.
For further orders, he picks the cheapest non-alcoholic beverage in the menu. The order might
be equal to previous orders. (3-letter code – spc)

**4. Alcoholic Customer** – This is a customer who only orders alcoholic beverages. He starts with
ordering the cheapest one, and in each further order, he picks the next expensive alcoholic
beverage. After reaching the most expensive alcoholic beverage, he won't order again. (3-
letter code – alc)

**Notes:** 
- If a customer cannot complete his order (for example – A vegetarian customer tries to
order but the menu has no vegetarian dish), he won't order at all.
- When the strategy is ordering the “most expensive” or “cheapest” dish, and there is
more than one such dish, then the dish with the smallest id will be ordered
