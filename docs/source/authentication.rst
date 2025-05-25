Authentication
=================

auth_token Parameter
--------------------

The `auth_token` parameter in `dequest` is used to authenticate API requests by adding a Bearer token to the request's `Authorization` header.

Overview
--------


Usage
-----

The `auth_token` parameter accepts either:

- A **string** containing a static Bearer token.
- A **callable** (e.g., a function) that returns the token string dynamically at runtime.

In both cases, `dequest` will automatically add the token to the `Authorization` header in the format:

.. code-block:: text

   Authorization: Bearer <your_token_here>

Examples
--------

**Static Token**

If you have a fixed token, you can provide it directly as a string:

.. code-block:: python

   from dequest import sync_client

   @sync_client(
       url="https://api.example.com/data",
       auth_token="your_static_token"
   )
   def get_data():
       pass

This will send the following header with every request:

.. code-block:: text

   Authorization: Bearer your_static_token

**Dynamic Token**

To generate the token at runtime (e.g., for refreshing tokens), pass a callable instead:

.. code-block:: python

   from dequest import async_client, HttpMethod

   def get_access_token():
       # Logic to fetch or refresh access token
       return "your_dynamic_token"

   @async_client(
       url="https://api.example.com/protected",
       method=HttpMethod.GET,
       auth_token=get_access_token
   )
   def fetch_protected_data():
       pass

In this case, `get_access_token()` will be called every time a request is made, and the returned token will be included as a Bearer token in the `Authorization` header.

api_key Parameter (x-api-key Header)
------------
The `api_key` parameter in `dequest` is used to authenticate API requests by adding an API key to the request's headers. It is particularly useful for APIs that require API keys by adding a token to the request's `x-api-key` header.


Usage
--------

The `api_key` parameter accepts either:
- A **string** containing the API key.
- A **callable** (e.g., a function) that returns the API key string dynamically at runtime.
- 
In both cases, `dequest` will automatically add the key to the request headers in the format:

.. code-block:: text

   x-api-key: <your_api_key_here>

Examples
--------

**Static API Key**

If you have a fixed API key, you can provide it directly as a string:

.. code-block:: python

   from dequest import sync_client

   @sync_client(
       url="https://api.example.com/data",
       api_key="your_static_api_key"
   )
   def get_data():
       pass

This will send the following header with every request:

.. code-block:: text

   x-api-key: your_static_api_key

**Dynamic API Key**

To generate the API key at runtime (e.g., for rotating keys), pass a callable instead:

.. code-block:: python

   from dequest import async_client, HttpMethod

   def get_api_key():
       # Logic to fetch or refresh API key
       return "your_dynamic_api_key"

   @async_client(
       url="https://api.example.com/protected",
       method=HttpMethod.GET,
       api_key=get_api_key
   )
   def fetch_protected_data():
       pass

In this case, `get_api_key()` will be called every time a request is made, and the returned key will be included in the `x-api-key` header.
This allows for dynamic key management.