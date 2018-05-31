.. _JsonPath: http://goessner.net/articles/JsonPath/
.. _ECMAScript 2015 (ES6): http://www.ecma-international.org/ecma-262/6.0/

Reference
=========

Runtime Settings
----------------

Runtime settings define common configuration parameters on how it should be executed.

Parameters
^^^^^^^^^^

**Version**

    | Type: String
    | Required: No
    | Default: 1.0
    | Description: Engine version number.

**Name**

    | Type: String
    | Required: No
    | Example: "My Integration Config"
    | Description: Configuration file name.

**LogPayload**

    | Type: Boolean
    | Required: No
    | Default: false
    | Example: true
    | Description: Determines whether payload (input and output) should be logged. It is recommended to do so to help with debugging. Some integrations may use up a lot of disk however.

**EntryPipelineId**

    | Type: String
    | Required: Yes
    | Example: Pipeline1
    | Description: Starting point of execution.

Connection
----------

Connection represents a physical connection to a local or a remote resource. Following sections describe supported
connection types in more details.

RESTful
^^^^^^^

Connection definition for HTTP RESTful API. A RESTful API is an application program interface (API) that uses HTTP 
requests to GET, PUT, POST and DELETE data.

Parameters
""""""""""

**Type**

    | Type: String
    | Required: Yes
    | Accepts: ``Restful``, ``Datum``
    | Description: Connection type.

**Name**

    | Type: String
    | Required: Yes
    | Example: My connection name
    | Description: Connection name. It will be referenced in logs.

**Url**

    | Type: String
    | Required: Yes
    | Validation: Must be valid URL
    | Example: \https://api.my-company.com/v1
    | Description: Connection base URL
    
**Authentication**

    | Type: Integer
    | Required: Yes
    | Example: 5
    | Description: Id of Authentication entity. Must be created in Integration > Authentication form.

Datum
^^^^^

Datum is the name of the internal Subscribe-HR data layer. It is used to perform direct read and write operations from 
and to Subscribe-HR. Unlike API connection Datum is fast and does not require additional parameters or authentication
to be defined.

Parameters
""""""""""

**Type**

    | Type: String
    | Required: Yes
    | Accepts: ``Restful``, ``Datum``
    | Description: Connection type.

**Name**

    | Type: String
    | Required: Yes
    | Example: My connection name
    | Description: Connection name. It will be referenced in logs.

Operation
---------

Operation represents an action that can be performed through a given connection. For example a GET requests
through RESTful API or query operation through a database connection.

RESTful
^^^^^^^

Operation definition for HTTP RESTful API. Must reference RESTful connection.

Parameters
""""""""""

**Name**

    | Type: String
    | Required: Yes
    | Example: My read operation
    | Description: Operation name. It will be referenced in logs.

**Connection**

    | Type: String
    | Required: Yes
    | Example: MyRestfulConnection
    | Description: Connection reference key that was used in the definition.

**Method**

    | Type: String
    | Required: Yes
    | Accepts: ``GET``, ``POST``, ``PUT``, ``PATCH``, ``DELETE``
    | Example: GET
    | Description: HTTP request method.

**Path**

    | Type: String
    | Required: Yes
    | Example: /api/v1/employees/:Id
    | Description: HTTP end point path. Supports merge parametrs e.g. :Id. Will be merged from Paramers.Url when passed in input.

**ErrorHandlingStyle**

    | Type: String
    | Required: No
    | Accepts: ``warn``, ``halt``
    | Default: halt
    | Example: warn
    | Description: Indicates how to handle errors when they are encountered. For example if operation received an error when trying to write data ``warn`` will produce a warning and continue execution of next record. Halt will completely terminate the process.

**ErrorStatusCodes**

    | Type: Array
    | Required: No
    | Default: ['4xx', '5xx']
    | Example: [400, '5xx']
    | Description: HTTP status codes that constitue errors. Supports wild card declaration e.g. ``5xx`` will include all 500 error codes.

**ErrorStatusCodeExceptions**

    | Type: Array
    | Required: No
    | Default: [404]
    | Example: [403, 404]
    | Description: HTTP status codes that should be ignored even if they are defined in ErrorStatusCodes field. Supports wild card declaration e.g. ``5xx`` will include all 500 error codes.

**OutputType**

    | Type: String
    | Required: No
    | Accepts: ``json``, ``raw``
    | Default: json
    | Example: json
    | Description: Output format to expect operation to return.

Datum
^^^^^

Operation definition for Datum connection type. Must reference Datum connection.

Parameters
""""""""""

**Name**

    | Type: String
    | Required: Yes
    | Example: My read operation
    | Description: Operation name. It will be referenced in logs.

**Connection**

    | Type: String
    | Required: Yes
    | Example: MyDatumConnection
    | Description: Connection reference key that was used in the definition.

**Action**

    | Type: String
    | Required: Yes
    | Accepts: ``Create``, ``Update``, ``Get``, ``Delete``, ``Query``
    | Example: Get
    | Description: Type of operation to perform.

**Entity**

    | Type: String
    | Required: :guilabel:`Yes unless Action is Query`
    | Validation: Valid Subscribe-HR entity name
    | Example: Employee
    | Description: Subscribe-HR entity name. Can be found in Development > Objects > System Name.

**Query**

    | Type: String
    | Required: :guilabel:`Yes only if Action is Query`
    | Validation: Valid SSQL query
    | Example: SELECT e FROM Employee e
    | Description: Must provide full SSQL query if ``Action is Query``. Otherwise only where clause condition e.g. ``Id = :Id``.

**ErrorHandlingStyle**

    | Type: String
    | Required: No
    | Accepts: ``warn``, ``halt``
    | Default: halt
    | Example: warn
    | Description: Indicates how to handle errors when they are encountered. For example if operation received an error when trying to write data ``warn`` will produce a warning and continue execution of next record. Halt will completely terminate the process.

**Pagination**

    | Type: Object (`Pagination`_)
    | Required: No
    | Example: ``{ MaxItemsPerPage: 10 }``
    | Description: Pagination parameters allow splitting requests into batches. There may be cases where a lot of data needs to be processed at once. Due to resource allocation limits it is best practice to paginate your operations to return 100 or less records at a time. The limit is not enforced. This may change in the future. Pagination parameter can only be used with Action of type ``Get`` and ``Query``.

**OutputType**

    | Type: String
    | Required: No
    | Accepts: ``json``, ``raw``
    | Default: json
    | Example: json
    | Description: Output format to expect operation to return.

Pagination
----------

Pagination defines how operations paginate requests.

Datum
^^^^^

Pagination definition for Datum operations.

Parameters
""""""""""

**MaxItemsPerPage**

    | Type: Integer
    | Required: Yes
    | Example: 100
    | Description: Number of records to return per page.

Mapping
-------

Mapping defines how source field is transformed into destination field. Mappings utilise `JsonPath`_ syntax to perform transformations making it very flexible to manipulate data. Mappings only deal with JSON format. Any operation that returns any other type of data will need to be converted into JSON first.

Parameters
^^^^^^^^^^

**FromField**

    | Type: String
    | Required: :guilabel:`Yes if no Default specified`
    | Example: $.id
    | Description: JsonPath pattern to extract source field value.

**ToField**

    | Type: String
    | Required: Yes
    | Example: $.Data.Surname
    | Description: JsonPath pattern for destination field.

**Tag**

    | Type: String
    | Required: No
    | Example: MyEarlierTag
    | Description: Perform mapping from a data tag instead of input data.

**Default**

    | Type: String
    | Required: :guilabel:`Yes if no FromField specified`
    | Example: Some string
    | Description: Default value to assign to a field if from field is NULL or an empty string. Alternatively if FromField is not specified default value will be written into destination field.

**Translations**

    | Type: Object
    | Required: No
    | Example: ``{"m": "male"}``
    | Description: Transformation object if field needs to be transformed to a different value. For example source value may be set to ``m`` which should be translated into ``male`` in destination field.

**DateFormatFrom**

    | Type: String
    | Required: :guilabel:`Yes if DateFormatTo is specified`
    | Example: d-m-Y
    | Description: For date fields indicates what format to expect dates in.

**DateFormatTo**

    | Type: String
    | Required: No
    | Example: Y-m-d
    | Description: For date fields indicates what format to output dates into.

.. note:: If only DateFormatFrom attribute is specified the default output format will be set to ``Y-m-d H:i:s``.

**FunctionId**

    | Type: Object (`Function`_)
    | Required: No
    | Example: MyMappingFunction
    | Description: Reference to predefined function to use to perform the mapping.

**Code**

    | Type: Function
    | Required: No
    | Example: function(input) { const output = input; return output; }
    | Description: Inline function definition to perform the mapping.

Function
--------

Users can define javascript functions to use during pipeline execution. 
The syntax is ``function(input) { const output = input; return output; }``. Javascript engine that we use behind the 
scenes is v8 which has support for `ECMAScript 2015 (ES6)`_.

Logical
^^^^^^^

Logical functions control the flow of pipeline execution process and determine what pipelines should be executed next 
based on the input. One of the most common use cases will be to determine if API has returned a status code of 404 and
then execute record creation pipeline otherwise perform an update. Expected output of the function is a string with 
a pipeline name or an array with multiple names to be executed next.

Parameters
""""""""""

**Type**

    | Type: String
    | Required: Yes
    | Accepts: ``Logical``, ``Mapping``
    | Example: Logical
    | Description: Defines function type.

**Code**

    | Type: Function
    | Required: Yes
    | Validation: Valid javascript function
    | Example: function(input) { if (input.StatusCode == "404") { return "Pipeline2"; } return "Pipeline3"; }
    | Description: Function code.

Mapping
^^^^^^^

Mapping functions are used to assist with performing mapping operations. Input comes from a selector or a previous 
action. Function can then manipulate the input and produce an output that is then passed back to the execution action.

Parameters
""""""""""

**Type**

    | Type: String
    | Required: Yes
    | Accepts: ``Logical``, ``Mapping``
    | Example: Mapping
    | Description: Defines function type.

**Code**

    | Type: Function
    | Required: Yes
    | Validation: Valid javascript function
    | Example: function(input) { if (input.EmploymentType == "f") { return "fulltime"; } }
    | Description: Function code.

Pipeline
--------

Pipeline is a list of actions that need to be performed in a sequence. Pipelines can be nested and trigger other 
pipelines. There are a number of action types that can be performed within pipeline which are onlined below.

Common Action Parameters
^^^^^^^^^^^^^^^^^^^^^^^^

**Type**

    | Type: String
    | Required: Yes
    | Accepts: ``Operation``, ``Iterator``, ``Map``, ``Function``, ``Pipeline``
    | Example: Operation
    | Description: Defines action type to execute.

**InputTag**

    | Type: String
    | Required: No
    | Example: MyEmployeeRecord
    | Description: Tags input data to reuse later on in the pipeline.

**OutputTag**

    | Type: String
    | Required: No
    | Example: MyEmployeeOutputRecord
    | Description: Tags output data to reuse later on in the pipeline.

Operation Action
^^^^^^^^^^^^^^^^

Triggers an operation.

Parameters
""""""""""

Inherits all `Common Action Parameters`_

**Id**

    | Type: String
    | Required: Yes
    | Example: MyApiGetOperation
    | Description: Operation Id to execute.

Iterator Action
^^^^^^^^^^^^^^^

Iterates over a data set.

Parameters
""""""""""

Inherits all `Common Action Parameters`_

**Selector**

    | Type: String
    | Required: Yes
    | Example: $.Data.content[*]
    | Description: JsonPath expression to indicate which data to iterate over.

Map Action
^^^^^^^^^^

Iterates over a data set.

Parameters
""""""""""

Inherits all `Common Action Parameters`_

**Id**

    | Type: String
    | Required: Yes
    | Example: MyEmployeeMappings
    | Description: Mapping Id to execute.

Function Action
^^^^^^^^^^^^^^^

Executes a javascript function. Can either be an inline function or reference to a predefined function.

Parameters
""""""""""

Inherits all `Common Action Parameters`_

**Id**

    | Type: String
    | Required: :guilabel:`Yes if using a predefined function`
    | Example: MyPredefinedFunction1
    | Description: Predefined function Id.

**FunctionType**

    | Type: String
    | Required: :guilabel:`Yes if it is an inline function`
    | Accepts: ``Logical``, ``Mapping``
    | Example: MyPredefinedFunction1
    | Description: Predefined function Id.

**Code**

    | Type: String
    | Required: :guilabel:`Yes if it is an inline function`
    | Example: function(input) { const output = input; return output; }
    | Description: Inline function definition.

.. _pipeline-action-ref:

Pipeline Action
^^^^^^^^^^^^^^^

Executes a pipeline or a series of pipelines.

..  note:: 
    Pipeline action must be the last action in the sequence. It is not possible to return output from pipeline 
    and continue executing another action. This design ensures that there is no retrace within execution plan to minimise
    errors and keep pipelines linear.

Parameters
""""""""""

Inherits all `Common Action Parameters`_

**Id**

    | Type: String or Array
    | Required: Yes
    | Example: ["Pipeline2", "Pipeline3"]
    | Description: One or multiple pipelines to execute.









 