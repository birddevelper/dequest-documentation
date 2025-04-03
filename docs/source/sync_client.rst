Using `sync_client`
===================

The `@sync_client` decorator allows you to make synchronous HTTP requests declaratively.

Basic Example
-------------

.. code-block:: python

   from dequest import sync_client

   @sync_client(url="https://api.example.com/users")
   def get_users():
       pass  # No implementation needed!

   users = get_users()
   print(users)

This sends a GET request and automatically handles response parsing.
