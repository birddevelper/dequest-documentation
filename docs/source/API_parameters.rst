API Parameters
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


In endpoint function definitions, `QueryParameter`, `PathParameter`, `FormParameter`, and `JsonBody` parameters support two optional arguments:

1. **Type Hint (First Argument)**  
   
   You can provide a type hint (e.g., `str`, `int`, `bool`, etc.) as the first argument. This enables automatic type checking and validation before making the API call.

   **Example:**

.. code-block:: python

   keyword: QueryParameter[str]

This ensures that `keyword` must be a string when passed to the function.

2. **Mapping Name (Second Argument)**  
   The second argument is an optional string that lets you map the Python parameter name to the actual API parameter name.

   **Example:**

.. code-block:: python

   keyword: QueryParameter[str, "q"]

This maps the `keyword` parameter in your function to the `q` query parameter in the API request.

These features help keep your code type-safe and aligned with external API schemas. Note that each of these arguments can be used alone.
