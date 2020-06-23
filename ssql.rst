SSQL
====

Overview
--------

Subscribe-HR Structured Query Language is a subset of SQL language which is used to query data within Subscribe-HR 
object relational model. Queries can be performed using either server side data access module or RESTful API 
query end point.

SSQL Terminology
----------------

The following terminology will be used when referencing SSQL statements. SSQL left, SQL right.

-   Entity / Object - table
-   Field - column

Supported Expressions
---------------------

Statement Types
+++++++++++++++

-   SELECT
-   INSERT
-   UPDATE
-   DELETE

Joins
+++++

-   JOIN / INNER JOIN
-   LEFT JOIN
-   RIGHT JOIN
-   CROSS JOIN

Operators
+++++++++

+---------------------+-------------------------------------------------------------------------+
| Operator            | Description                                                             |
+=====================+=========================================================================+
| =                   | Comparison equal operator                                               |
+---------------------+-------------------------------------------------------------------------+
| <=>                 | | NULL-safe equal. This operator performs an equality comparison like   |
|                     | | the = operator, but returns 1 rather than NULL if both operands are   |
|                     | | NULL, and 0 rather than NULL if one operand is NULL                   |
+---------------------+-------------------------------------------------------------------------+
| >                   | Greater than                                                            |
+---------------------+-------------------------------------------------------------------------+
| >=                  | Greater than or equals to                                               |
+---------------------+-------------------------------------------------------------------------+
| <                   | Less than                                                               |
+---------------------+-------------------------------------------------------------------------+
| <=                  | Less than or equals to                                                  |
+---------------------+-------------------------------------------------------------------------+
| <>                  | Not equal                                                               |
+---------------------+-------------------------------------------------------------------------+
| LIKE                | Uses wildcard operators to compare a value to similar values.           |
+---------------------+-------------------------------------------------------------------------+
| NOT                 | Reverses the meaning of the logical operator e.g. NOT IN                |
+---------------------+-------------------------------------------------------------------------+
| AND                 | It allows the existence of multiple conditions                          |
+---------------------+-------------------------------------------------------------------------+
| OR                  | It is used to combine multiple conditions                               |
+---------------------+-------------------------------------------------------------------------+
| IN                  | It is used to compare a value in a list of literal values               |
+---------------------+-------------------------------------------------------------------------+
| IS                  | Tests a value against a boolean value e.g. IS NULL                      |
+---------------------+-------------------------------------------------------------------------+
| \+                  | Addition                                                                |
+---------------------+-------------------------------------------------------------------------+
| \-                  | Subtraction                                                             |
+---------------------+-------------------------------------------------------------------------+
| \/                  | Division                                                                |
+---------------------+-------------------------------------------------------------------------+
| \*                  | Multiplication                                                          |
+---------------------+-------------------------------------------------------------------------+

Functions
+++++++++

+---------------------+-------------------------------------------------------------------------+--------------------------------+
| Function            | Description                                                             | Example                        |
+=====================+=========================================================================+================================+
| ABS                 | Returns the absolute value of a number                                  | | ABS(-5)                      |
|                     |                                                                         | | Result: 5                    |
+---------------------+-------------------------------------------------------------------------+--------------------------------+
| CEIL                | | Returns the smallest integer value not less than the number specified | | CEIL(1.2)                    |
|                     | | as an argument                                                        | | Result: 2                    |
+---------------------+-------------------------------------------------------------------------+--------------------------------+
| FLOOR               | | Returns the largest integer value not greater than the number         | | FLOOR(1.2)                   |
|                     | | specified as an argument                                              | | Result: 1                    |
+---------------------+-------------------------------------------------------------------------+--------------------------------+
| SQRT                | Square root                                                             | | SQRT(25)                     |
|                     |                                                                         | | Result: 5                    |
+---------------------+-------------------------------------------------------------------------+--------------------------------+
| MOD                 | Returns the remainder of dividend divided by divisor                    | | MOD(17,5)                    |
|                     |                                                                         | | Result: 2                    |
+---------------------+-------------------------------------------------------------------------+--------------------------------+
| LENGTH              | Returns the length of a string                                          | | LENGTH('hi')                 |
|                     |                                                                         | | Result: 2                    |
+---------------------+-------------------------------------------------------------------------+--------------------------------+
| SUBSTRING           | Extract a substring from a string                                       | | SUBSTRING('Subscribe', 2, 5) |
|                     |                                                                         | | Result: ubscr                |
+---------------------+-------------------------------------------------------------------------+--------------------------------+
| LOWER               | Convert the text to lower-case                                          | | LOWER('HI')                  |
|                     |                                                                         | | Result: hi                   |
+---------------------+-------------------------------------------------------------------------+--------------------------------+
| UPPER               | Convert the text to upper-case                                          | | UPPER('hi')                  |
|                     |                                                                         | | Result: HI                   |
+---------------------+-------------------------------------------------------------------------+--------------------------------+
| CONCAT              | Adds several strings together                                           | | CONCAT('h', 'i')             |
|                     |                                                                         | | Result: hi                   |
+---------------------+-------------------------------------------------------------------------+--------------------------------+
| COALESCE            | Returns the first non-null value in a list                              | | COALESCE(NULL, 1)            |
|                     |                                                                         | | Result: 1                    |
+---------------------+-------------------------------------------------------------------------+--------------------------------+

Aggregate Function
++++++++++++++++++

+---------------------+-------------------------------------------------------------------------+
| Function            | Description                                                             |
+=====================+=========================================================================+
| AVG                 | Average                                                                 |
+---------------------+-------------------------------------------------------------------------+
| COUNT               | Count a number of records in a group                                    |
+---------------------+-------------------------------------------------------------------------+
| MAX                 | Maximum value in a set                                                  |
+---------------------+-------------------------------------------------------------------------+
| MIN                 | Minimum value in a set                                                  |
+---------------------+-------------------------------------------------------------------------+
| STD                 | Standard deviation                                                      |
+---------------------+-------------------------------------------------------------------------+
| SUM                 | Sum of values in a group                                                |
+---------------------+-------------------------------------------------------------------------+
| VARIANCE            | Population standard variance                                            |
+---------------------+-------------------------------------------------------------------------+
| GROUP_CONCAT        | Returns a string with concatenated non-NULL value from a group          |
+---------------------+-------------------------------------------------------------------------+

Object Relationships
--------------------

Subscribe-HR objects and fields can be created dynamically using development tool. These objects and fields 
can then be queried using SSQL. Objects can also have parent child relationships. One parent object can have many 
child objects related to it. At the same time, a child can only have one parent. For performance reasons no 
nested relationships are allowed. 

..  note:: 
    To find object names go to Development > Objects. You will see Object System Name column.

Common Fields
-------------

The following fields are common for every object.

+---------------------+-------------------------------------------------------------------------+
| Field Name          | Description                                                             |
+=====================+=========================================================================+
| Id                  | Unique record Id                                                        |
+---------------------+-------------------------------------------------------------------------+
| CreatedBy           | Creator user Id                                                         |
+---------------------+-------------------------------------------------------------------------+
| CreatedDate         | Date when record was first created                                      |
+---------------------+-------------------------------------------------------------------------+
| LastModifiedBy      | User id that last modified the record                                   |
+---------------------+-------------------------------------------------------------------------+
| LastModifiedDate    | Date when record was last modified                                      |
+---------------------+-------------------------------------------------------------------------+
| __ParentId          | Foreign key (only child objects)                                        |
+---------------------+-------------------------------------------------------------------------+

Learning By Example
-------------------

Simple Statement
++++++++++++++++

.. code-block:: sql

    SELECT e FROM Employee e WHERE e.FirstName = 'Maria';

Return all employees with the first name Maria.

..  note:: 
    Select * (star) expression is not supported. You must specify list of aliases or field names.

..  note:: 
    All entities in from clause must have an alias.

Join Statement
++++++++++++++

.. code-block:: sql

    SELECT e, ea FROM Employee e LEFT JOIN EmployeeAddress ea ON (e.Id = ea.__ParentId) WHERE e.FirstName = 'Maria';

Return all employees with the first name Maria and their addresses.

