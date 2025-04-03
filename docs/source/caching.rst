Caching Responses
=================

To improve performance, `dequest` can cache responses.

.. code-block:: python

   @sync_client(url="https://api.example.com/popular", enable_cache=True, cache_ttl=60)
   def get_popular():
       pass

The response will be stored for 60 seconds, reducing API calls. It generates a cache_key by combining the URL, including the values of path parameters, with query parameters, ensuring that an API request with different query parameters is cached separately for each paramter set.