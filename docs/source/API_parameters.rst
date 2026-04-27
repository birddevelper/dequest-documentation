API Parameters
===================

`dequest` provides `PathParameter`, `QueryParameter`, `FormParameter`, and `JsonBody` to handle API parameters.

Path Parameters
---------------

Use `PathParameter` to include values in the URL:

.. code-block:: python

   from dequest import sync_client, PathParameter

   @sync_client(url="https://api.example.com/users/{user_id}")
   def get_user(user_id: int = PathParameter()):
       pass

   user = get_user(user_id=42)
   print(user)

Query Parameters
----------------

Use `QueryParameter` to pass values as query parameters:

.. code-block:: python

   from dequest import sync_client, QueryParameter

   @sync_client(url="https://api.example.com/search")
   def search(keyword: str = QueryParameter(alias="q")):
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
       full_name: str = FormParameter(alias="name"),
       grade: int = FormParameter(),
       city: str = FormParameter(),
       birthday: str = FormParameter(),
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
       name: str = JsonBody(),
       grade: int = JsonBody(),
       city_name: str = JsonBody(alias="city"),
       birthday: str = JsonBody(),
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

Parameter Declaration Style
---------------------------

`dequest` now follows a FastAPI-style parameter declaration pattern:

.. code-block:: python

   full_name: str = FormParameter(alias="name")
   keyword: str = QueryParameter(default="python", alias="q")
   user_id: int = PathParameter()

In this style:

1. The Python type comes from the normal annotation such as `str`, `int`, or `bool`.
2. Parameter options such as `alias` and `default` are passed to `PathParameter(...)`, `QueryParameter(...)`, `FormParameter(...)`, or `JsonBody(...)`.
3. If a default value is provided in the parameter marker, that value is used when the function is called without that argument.

Deprecated Subscription Style
-----------------------------

The old subscription-based declaration style is deprecated and will be removed in a future release.

Deprecated style:

.. code-block:: python

   from dequest import sync_client, QueryParameter

   @sync_client(url="https://api.example.com/search")
   def search(keyword: QueryParameter[str, "q"]):
       pass

Use this instead:

.. code-block:: python

   from dequest import sync_client, QueryParameter

   @sync_client(url="https://api.example.com/search")
   def search(keyword: str = QueryParameter(alias="q")):
       pass

When the deprecated subscription style is used, `dequest` emits a `FutureWarning` to help identify code that should be migrated.
