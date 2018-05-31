JsonPath
========

JsonPath is an XPath-like expression language for filtering, flattening and extracting data. JsonPath uses special 
notation to represent nodes and their connections to adjacent nodes in a JsonPath. Tables below outline syntax and
provide some examples on how to apply it in transformations. JsonPath is used in iterators and mappings to help find 
and transform information. Most of the time it is sufficient to use JsonPath without applying functions to manipulate 
data because of its reach functionality.

Expression Syntax
^^^^^^^^^^^^^^^^^

+---------------------+------------------------------------------------------+
| Symbol              | Description                                          |
+=====================+======================================================+
| $                   | The root object/element (not strictly necessary)     |
+---------------------+------------------------------------------------------+
| @                   | The current object/element                           |
+---------------------+------------------------------------------------------+
| . or []             | Child operator                                       |
+---------------------+------------------------------------------------------+
| \.\.                | Recursive descent                                    |
+---------------------+------------------------------------------------------+
| \*                  | Wildcard. All child elements regardless their index. |
+---------------------+------------------------------------------------------+
| [,]	              | Array indices as a set                               |
+---------------------+------------------------------------------------------+
| [start\:end\:step]  | Array slice operator borrowed from ES4/Python.       |
+---------------------+------------------------------------------------------+
| ?()	            | Filters a result set by a script expression            |
+---------------------+------------------------------------------------------+
| ()	            | Uses the result of a script expression as the index    |
+---------------------+------------------------------------------------------+

Example
^^^^^^^

.. code-block:: json

    { 
        "store": 
        {
            "book": 
            [
                { 
                    "category": "reference",
                    "author": "Nigel Rees",
                    "title": "Sayings of the Century",
                    "price": 8.95,
                    "available": true
                },
                { 
                    "category": "fiction",
                    "author": "Evelyn Waugh",
                    "title": "Sword of Honour",
                    "price": 12.99,
                    "available": false
                },
                { 
                    "category": "fiction",
                    "author": "Herman Melville",
                    "title": "Moby Dick",
                    "isbn": "0-553-21311-3",
                    "price": 8.99,
                    "available": true
                },
                { 
                    "category": "fiction",
                    "author": "J. R. R. Tolkien",
                    "title": "The Lord of the Rings",
                    "isbn": "0-395-19395-8",
                    "price": 22.99,
                    "available": false
                }
            ],
            "bicycle": 
            {
                "color": "red",
                "price": 19.95,
                "available": true
            }
        },
        "authors": 
        [
            "Nigel Rees",
            "Evelyn Waugh",
            "Herman Melville",
            "J. R. R. Tolkien"
        ]
    }

+--------------------------------------------------+---------------------------------------------------+
| JsonPath                                         | Result                                            |
+==================================================+===================================================+
| $.store.bicycle.price                            | All books.                                        |
+--------------------------------------------------+---------------------------------------------------+
| $.store.book[*]                                  | The current object/element                        |
+--------------------------------------------------+---------------------------------------------------+
| $.store.book[1,3]                                | The second and fourth book.                       |
+--------------------------------------------------+---------------------------------------------------+
| $.store.book[1\:3]                               | From the second book to the fourth.               |
+--------------------------------------------------+---------------------------------------------------+
| $.store.book[\:2]                                | From the first book to the third.                 |
+--------------------------------------------------+---------------------------------------------------+
| $.store.book[x\:y\:z]	                           | Books from x to y with a step of z.               |
+--------------------------------------------------+---------------------------------------------------+
| [start\:end\:step]                               | Array slice operator borrowed from ES4/Python.    |
+--------------------------------------------------+---------------------------------------------------+
| $\.\.book[?(\@.category == 'fiction')]           | All books with category == 'fiction'.             |
+--------------------------------------------------+---------------------------------------------------+
| $\.\.\*[?(\@.available == true)].price           | All prices of available products.                 |
+--------------------------------------------------+---------------------------------------------------+
| $\.\.book[?(\@.price < 10)].title                | The title of all books with price lower than 10.  |
+--------------------------------------------------+---------------------------------------------------+
| $\.\.book[?(\@.author==$.authors[3])]            | All books by "J. R. R. Tolkien"                   |
+--------------------------------------------------+---------------------------------------------------+
| $[store]                                         | The store.                                        |
+--------------------------------------------------+---------------------------------------------------+
| $\.\.book[\*][title, 'category', "author"]       | title, category and author of all books.          |
+--------------------------------------------------+---------------------------------------------------+
