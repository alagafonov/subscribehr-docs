Authentication
==============

TO DO

.. note::

    Our code base is still maturing and the core team doesn't yet recommend
    running this as a pre-commit hook due to the number of changes this will
    cause while constructing a pull request. Independent pull requests with
    linting changes would be a great help to making this possible.

.. warning:: We currently do not provide caching on this API. 
             If the remote source you are including changes their page structure or deletes the content,
             your embed will break.

.. code-block:: javascript

    {
        a: "b"
    }

.. code:: bash

    pip install sphinx_rtd_theme