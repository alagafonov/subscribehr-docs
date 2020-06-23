Getting Started
====================

Introduction
------------

Welcome to Subscribe-HR development platform. Use this guide to extend Subscribe-HR functionality by building apps, 
tasks, or develop any other type of extensions. Subscribe-HR platform provides a wide range of tools like \
:doc:`SSQL </ssql>` query language, search capabilities, built in RESTful API and much more to utilise when building your apps.

To App or not to App
--------------------

Before venturing on the journey of extending Subscribe-HR functionality you need to make a decision whether this new
functionaity is going to be an app available via app store or just an extension for a particular system or client.
It's important to make that decision early on as naming convention for fields and entities will be different. Global 
apps will have an app code added to the names to maintain uniqueness across the platform.

If you decide to buld a global app then it's important to ensure that **Belongs to App** field is always linked to
your app. Failing to set that field will result in app failure after installation.

Items that can be packaged into an app:

- Objects or Entities
- Elements or Fields
- Scripts
- Workflow Tasks
- System Tasks
- Labels
- Messages
- Components
- Authentication Records
- Integration Processes

Components
----------

You will find a lot of references to components throughout this documentation. What are components? They are small
(or sometimes not so small) pieces of functionality developed for a specific purpose. There are four types of \
components that can be created in Subscribe-HR.

-  **Tool** - application that sits outside of a module. Usually a custom tool to extend existing functionality or provide \ 
   additional capabilities.

-  **Widget** - a tile on the dashboard. Dashboard can have up to ten tiles dropped to a single bucket at a time. Each \
   tile can be an application with its own permissions and functionality.

-  **Wizard** - a stepped wizard to enhance usability of the system. WIzards are available from Start option on the dashboard. 
   Wizards can also be triggered from other places. For example widgets.

Under the hood each component uses the same core functionality therefore documentation will reference them as such unless
there is a specific function that is designed for a particular component type.

Component Architecture
----------------------

This provides a brief breakdown of component structure. More detailed examples will be provided in later chapters. 
There are six main parts:

-  **API** - Allows creation of RESTful API. See :doc:`Server Side API Reference </apps_platform_reference>` for 
   available functions.

-  **Template** - Template that gets loaded when the component is first initiated. Generated on the server side.

-  **Front End Javascript** - Heading says it all. :doc:`Front End API Reference </apps_front_end_reference>` for 
   available functions and libraries.

-  **CSS** - Stylesheet.

-  **Permissions** - JSON structure describing permissions required for this component.