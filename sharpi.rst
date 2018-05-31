Integration Tool (SHaRpi)
=========================

Our developers are always trying to come up with clever code names for dev tools and SHaRpi is no exception. 
SHaRpi is an integration app that allows linking of two or more APIs together using only a JSON template.
It also includes detailed logging and integration with common Subscribe-HR features like :doc:`SSQL </ssql>`.

The definition is a combination of independent blocks each with its own purpose. Blocks are then combined into a
pipeline which outlines where the data is coming from, how it should be transformed and where it should be saved. 
This documentation will provide full API reference for each block and include examples of real configurations.

.. toctree::
   :maxdepth: 2
   :caption: Sections

   sharpi_authentication
   sharpi_architecture
   sharpi_jsonpath
   sharpi_dateformat
   sharpi_reference
   sharpi_tutorial
