Server API Reference
====================

Classes
---------------------

.. _permission-manager:

PermissionManager
+++++++++++++++++++++

Provides access to user defined permissions for a component.

.. function:: hasPermission(code)

   Returns true if current user has access to specified permission code.

   :param code: (string) permission code
   :rtype: boolean

Modules
---------------------

Environment (Shr.Env)
+++++++++++++++++++++

The environment module provides access to a set of internal platform functions.

.. function:: Shr.Env.log(variable)

   Displays information about a variable in a readable way.

   :param variable: (mixed) variable to format
   :rtype: string

.. function:: Shr.Env.getBaseUrl()

   Returns system base URL (e.g. https://app.subscribe-hr.com).

   :rtype: string

.. function:: Shr.Env.getAppUrl()

   Returns application URL (e.g. https://app.subscribe-hr.com/cb/app).

   :rtype: string

.. function:: Shr.Env.getModuleUrl()

   Returns current module URL (e.g. https://app.subscribe-hr.com/cb/app/hr).

   :rtype: string

.. function:: Shr.Env.getComponentApiUrl(id, function, options)

   Creates request URL for component API function.

   :param id: (string) component id
   :param function: (string) function name
   :param options: (object) URL parameters
   :rtype: string

.. function:: Shr.Env.getComponentPermissions(id)

   Returns component permission manager.

   :param id: (string) component id
   :rtype: :ref:`PermissionManager <permission-manager>`

Request (Shr.Request)
+++++++++++++++++++++

Request module provides access to information about the client request. 
This information can be accessed via the following methods.

.. function:: Shr.Request.getParameter(name)

   Extracts client parameter from POST or GET request.

   :param name: (string) parameter name
   :rtype: mixed

UI / Template (Shr.UI)
++++++++++++++++++++++

UI module provides functions to help generate user interface.

.. function:: Shr.UI.createField(options)

   Generates form field in the template.

   :param options: (arguments) series of arguments depending on the type of field being generated
   :rtype: string

Util (Shr.Util.Base64)
++++++++++++++++++++++

Module to encode and decode base64 strings.

.. function:: Shr.Util.Base64.encode(content)

   Encodes string in base64 format.

   :param content: (string) string to encode
   :rtype: string

.. function:: Shr.Util.Base64.decode(content)

   Decodes a base64 string.

   :param content: (string) string to decode
   :rtype: string

Util (Shr.Util.File)
++++++++++++++++++++

Module to work with files Subscribe-HR virtual storage.

.. function:: Shr.Util.File.create(name, content, isTemp)

   Creates file in virtual storage.

   :param name: (string) file name
   :param content: (string) file content
   :param isTemp: (boolean) is file temporary (temporary files are removed after 24 hours if they are not attached to records)
   :rtype: string - file id

.. function:: Shr.Util.File.update(id, name, content)

   Update file.

   :param name: (string) file id
   :param name: (string) file name
   :param content: (string) file content
   :rtype: string - file id