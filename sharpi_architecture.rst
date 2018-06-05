.. _ECMAScript 2015 (ES6): http://www.ecma-international.org/ecma-262/6.0/

Architecture
============

SHaRpi is architected to be highly configurable to ensure that interfaces to third party products can be easily setup
and adjusted based on client requirements. Usually customers have unique needs in relation to data flow 
directions, field mappings, scheduling and accessibility of end points. By using SHaRpi these modifications are easily 
implemented in a configuration file rather than by performing complex customisations.

The architecture of the tool consists of a number of predefined blocks (or as we call them actions) each with its own 
purpose. Actions then get connected together to form a pipeline. Each action performs a unique task like extracting 
data from data source or transforming it into another format. Each action is logged into the Subscribe-HR logging
platform including input and output making it easy to debug.

The data format that is used to pass information between blocks is JSON. Some operations may not always be capable of 
returning JSON in which case conversion is performed. Below you will find examples of input and output data from
different action types.

Actions
^^^^^^^

The following table outlines all possible actions that can be defined within a pipeline. For detailed reference
refer to :ref:`pipeline-action-ref`.

+---------------------+------------------------------------------------------+
| Action              | Description                                          |
+=====================+======================================================+
| Operation           | Performs CRUD operation on a given connection        |
+---------------------+------------------------------------------------------+
| Iterator            | Iterates over a dataset                              |
+---------------------+------------------------------------------------------+
| Map                 | Transforms data from one format into another         |
+---------------------+------------------------------------------------------+
| Function            | Executes a javascript function                       |
+---------------------+------------------------------------------------------+
| Pipeline            | Triggers a pipeline                                  |
+---------------------+------------------------------------------------------+

Operation
^^^^^^^^^

Operations can be of different types and require different parameters to be passed into them. Return data can also vary.
For example with RESTful APIs it may be important to be able to access response headers as well as set request headers
based on information that was received from another operation. For a Datum connection however headers are not required
and it would not make sense to pass them in. Examples below show different formats for input and output for each action 
type.

.. _architecture-restful-input:

RESTful Input
"""""""""""""

**Parameters.Url** 
 
 | Will be merged into operation path which makes it possible to add parameters directly to URL. If path is set to ``/api/v1/employee/:Id`` the following input will change it to ``/api/v1/employee/100``. 

**Parameters.Form** 
 
 | Will be added as variables to request.

**Headers** 
 
 | Will be appended to request headers.

**Data** 
 
 | Sent in request body and encoded as JSON or supplied Content Type.

.. code-block:: json

    {
        "Parameters": {
            "Url": {
                "Id": "100"
            }
            "Form": {
                "Name": "Alex"
            }
        },
        "Headers": {
            "Content-Type": "application\/json"
        },
        "Data": [
            {
                "Id": 1,
                "Company": 1,
                "FirstName": "Alex"
            }
        ]
    }

.. _architecture-restful-output:

RESTful Output
""""""""""""""

**Headers** 
 
 | Response headers.

**StatusCode** 
 
 | Response status code.

**Status** 
 
 | Response status text.

**Data** 
 
 | Based on return format specified in operation.

.. code-block:: json

    {
        "Headers": {
            "Cache-Control": [
                "no-cache, must-revalidate"
            ],
            "Content-Type": [
                "application\/json"
            ],
            "Date": [
                "Sat, 26 May 2018 07:28:18 GMT"
            ],
            "Expires": [
                "0"
            ],
            "Server": [
                "Apache"
            ]
        },
        "StatusCode": 200,
        "Status": "OK",
        "Data": [
            {
                "Id": 1,
                "Company": 1,
                "FirstName": "Alex",
                "LastName": "Agafonov"
            }
        ]
    }

Datum Input
"""""""""""

**Parameters** 
 
 | Parameters to merge into query. For example if query is set to ``SELECT e FROM Employee e WHERE e.Id = :Id`` the following input will change it to ``SELECT e FROM Employee e WHERE e.Id = 71``.

**Data** 

 | Data to write into entity.

.. code-block:: json

    {
        "Parameters": {
            "Id": 300
        },
        "Data": [
            {
                "Surname": "Brounders",
                "FirstName": "Maria",
                "DateOfBirth": "1970-11-05",
                "EyeTest": "2013-01-08",
                "DateLeft": null,
                "EmploymentType": {
                    "Value": "fulltime"
                }
            }
        ]
    }

Datum Output
""""""""""""

**Data** 

 | Returns output of a query. Data for each entity is divided using entity name e.g. "Employee". If SSQL query is executed across multiple entities then multiple entity names are returned.

.. code-block:: json

    {
        "Data": [
            {
                "Employee": {
                    "Id": 71,
                    "CreatedBy": 4,
                    "CreatedDate": "2009-06-22T13:26:21+10:00",
                    "LastModifiedBy": 1,
                    "LastModifiedDate": "2018-05-18T09:07:03+10:00",
                    "Surname": "Brounders",
                    "FirstName": "Maria",
                    "DateOfBirth": "1970-11-05",
                    "EyeTest": "2013-01-08",
                    "DateLeft": null,
                    "EmploymentType": {
                        "Value": "fulltime",
                        "Text": "Full Time"
                    }
                },
                "EmployeeAddress": [
                    {
                        "Type": {
                            "Value": "residential",
                            "Text": "Residential"
                        },
                        "Address1": "..."
                    },
                    {
                        "Type": {
                            "Value": "postal",
                            "Text": "Postal"
                        },
                        "Address1": "..."
                    }
                ]
            }
        ]
    }

Iterator
^^^^^^^^

Iterator action will loop through the data. It can be thought of as standard for loop in programming. Selector
attribute will determine what data needs to be iterated over.

Conside the following example which is an output from Datum operation.

.. code-block:: json

    {
        "Data": [
            {
                "Employee": {
                    "Id": 71
                }
            },
            {
                "Employee": {
                    "Id": 72
                }
            },
            {
                "Employee": {
                    "Id": 73
                }
            }
        ]
    }

Assuming that iterator selector is set to ``$.Data[*]`` the first entry that will be returned is

.. code-block:: json

    {
        "Employee": {
            "Id": 71
        }
    }

Map
^^^

Map action will receive an input perform data transformation based on mappings specified and return transformed data 
structure.

Consider the following example which is a sample output from another operation.

.. code-block:: json

    {
        "Data": {
            "Employee": {
                "Id": 71
            }
        }
    }

The following transformation will then be applied.

.. code-block:: json

    {
        "SampleMappings": [
            {
                "FromField": "$.Data.Employee.Id",
                "ToField": "$.Result.EmployeeCode",
            }
        ]
    }

Which will result in the following output.

.. code-block:: json

    {
        "Result": {
            "EmployeeCode": 71
        }
    }

Function
^^^^^^^^

Functions add ability to perform complex mapping logic or make routing decisions based on output that is returned from 
previous operation. There are two different function types that can be used. Logical for routing and mapping for 
transforming data. Functions can be created inline or predefined and then called inside a pipeline. Function language 
is javascript. Engine that we use behind the scenes is v8 which supports most of `ECMAScript 2015 (ES6)`_. More
information on each function type is available below.

Logical Function
""""""""""""""""

Logical functions are designed to make complex routing decisions based on input data. 

Consider the following example of predefined function

.. code-block:: javascript

    {
        "DecisionFunction1": {
            "Type": "Logical",
            "Code": function(input) { 
                        if (input.Data.length == 0) {
                            return "Pipeline2";
                        }
                        return "Pipeline3";
                    }
        }
    }

If input is

.. code-block:: json

    {
        "Data": []
    }

Then function will return "Pipeline2" otherwise "Pipeline3". Putting it into simple terms, if Data element is empty 
then execution will move on to Pipeline2 otherwise it will trigger Pipeline3. It is also possible to return array with 
multiple pipelines e.g. ``["Pipeline4", "Pipeline5"]`` which will then execute two pipelines.

Mapping Function
""""""""""""""""

Mapping functions are designed to make complex transformations where it may not be sufficient to use JsonPath.

Consider the following example of inline function within mapping definition

.. code-block:: javascript

    {
        "SampleMappings": [
            {
                "FromField": "$.Data",
                "ToField": "$.Result.EmplType",
                "Code": function(input) {
                    if (input.EmploymentType === "f") {
                        return "FullTime";
                    }
                    return "PartTime";
                }
            }
        ]
    }

We will then pass in the following input

.. code-block:: json

    {
        "Data": {
            "EmploymentType": "f"
        }
    }

And get the following output

.. code-block:: json

    {
        "Result": {
            "EmplType": "FullTime"
        }
    }

Pipeline
^^^^^^^^

Pipeline actions are used to split execution into multiple streams either to make configuration files easier to read or
to create reusable pieces of logic. Pipeline take input from previous operation and continue executing actions 
defined within them.

..  note:: 
    Pipeline action must be the last action in the sequence. It is not possible to return output from pipeline 
    and continue executing another action. This design ensures that there is no retrace within execution plan to 
    minimise errors and keep pipelines linear.

Example of pipeline action definition

.. code-block:: json

    {
        "Type": "Pipeline",
        "Id": ["Pipeline43", "Pipeline2"],
    }
