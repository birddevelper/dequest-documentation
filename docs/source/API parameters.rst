Handling Parameters
===================

dequest provides `PathParameter`, `QueryParameter`, `FormParameter` and `JsonBody` to handle API parameters.

Path Parameters
---------------

Use `PathParameter` to include values in the URL:

.. code-block:: python

   from dequest import sync_client, PathParameter

   @sync_client(url="https://api.example.com/users/{user_id}")
   def get_user(user_id: PathParameter[int]):
       pass

   user = get_user(user_id=42)
   print(user)

Query Parameters
----------------

Use `QueryParameter` to pass values as query parameters:

.. code-block:: python

   from dequest import sync_client, QueryParameter

   @sync_client(url="https://api.example.com/search")
   def search(keyword: QueryParameter[str, "q"]):
       pass

   results = search(keyword="python")
   print(results)

