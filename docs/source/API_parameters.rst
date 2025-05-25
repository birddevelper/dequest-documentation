API Parameters
===================

`dequest` provides `PathParameter`, `QueryParameter`, `FormParameter`, and `JsonBody` to handle API parameters.

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

Form Parameters
---------------

Use `FormParameter` to send data as `application/x-www-form-urlencoded` in the body of a request. This is useful when working with APIs that require form-encoded input.

.. code-block:: python

   from dequest import sync_client, FormParameter

   @sync_client(
       url="https://api.example.com/users",
       dto_class=UserDTO,
       method="POST"
   )
   def save_user(
       full_name: FormParameter[str, "name"],  # maps "full_name" to "name" in the form body
       grade: FormParameter[int],
       city: FormParameter[str],
       birthday: FormParameter[str],
   ):
       pass

   save_user(
       full_name="Alice",
       grade=14,
       city="New York",
       birthday="2000-01-01"
   )

This sends the following form-encoded request body:

.. code-block:: text

   name=Alice&grade=14&city=New+York&birthday=2000-01-01

The `Content-Type` header is automatically set to `application/x-www-form-urlencoded`.

JSON Body Parameters
--------------------

Use `JsonBody` to send data in the body of a request as JSON. This is typically used with `POST`, `PUT`, or `PATCH` requests.

.. code-block:: python

   from dequest import sync_client, JsonBody

   @sync_client(
       url="https://api.example.com/users",
       dto_class=UserDTO,
       method="POST"
   )
   def save_user(
       name: JsonBody,
       grade: JsonBody,
       city_name: JsonBody["city"],  # maps to "city" in the request body
       birthday: JsonBody
   ):
       pass

   save_user(
       name="Alice",
       grade=14,
       city_name="New York",
       birthday="2000-01-01"
   )

This sends a JSON payload like:

.. code-block:: json

   {
     "name": "Alice",
     "grade": 14,
     "city": "New York",
     "birthday": "2000-01-01"
   }

The `Content-Type` is automatically set to `application/json`.

Optional Argument Support
-------------------------

In endpoint function definitions, `QueryParameter`, `PathParameter`, `FormParameter`, and `JsonBody` support two optional arguments:

1. **Type Hint (First Argument)**  
   You can provide a type hint (e.g., `str`, `int`, `bool`, etc.) as the first argument. This enables automatic type checking and validation before making the API call.

   **Example:**

   .. code-block:: python

      grade: FormParameter[int]

2. **Mapping Name (Second Argument)**  
   The second argument is an optional string that maps the Python parameter name to the actual parameter name expected by the API.

   **Example:**

   .. code-block:: python

      full_name: FormParameter[str, "name"]

This maps the `full_name` function parameter to the `name` field in the form data.

These features help keep your code type-safe and aligned with external API schemas. Each of these arguments can be used independently or together.
