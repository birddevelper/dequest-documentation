Using `async_client`
====================

The `@async_client` decorator lets you make non-blocking HTTP requests.

.. code-block:: python

   from dequest import async_client, HttpMethod

   @async_client(url="https://api.example.com/notify", method=HttpMethod.POST)
   def notify():
       pass

   notify()  # Fire-and-forget call


`async_client` makes asynchronous HTTP requests without requiring the user to handle async execution. It is designed for fire-and-forget scenarios, where you don't need to wait for the response.
The decorated function **should NOT** be awaited.

**url**: URL template with placeholders for path parameters.
**dto_class**: The DTO class to map the response data.
**source_field**: Source field to use for mapping response data. Leave None to map whole response.
**method**: HTTP method (GET, POST, PUT, DELETE).
**timeout**: Request timeout in seconds.
**retries**: Number of retries on failure.
**retry_on_exceptions**: Exceptions to retry on.
**retry_delay**: Delay in seconds between retries. Can be a static value or a function returning iterator.
**giveup**: Function to determine if the retry should be given up.
**auth_token**: Optional Bearer Token (static string or function returning a string).
**api_key**: Optional API key (static string or function returning a string).
**headers**: Optional default headers (can be a dict or a function returning a dict).
**enable_cache**: Whether to cache GET responses.
**cache_ttl**: Cache expiration time in seconds.
**circuit_breaker**: Instance of CircuitBreaker (optional).
**callback**: Optional function to process the response when available.
**consume**: Type of data to consume. ConsumerType.JSON, ConsumerType.XML or ConsumerType.TEXT



**Using Callbacks**

If you need to handle the response asynchronously, use a callback function. Callbacks let you process API responses asynchronously.

.. code-block:: python

   async def process_response(data):
       print("Received:", data)

   @async_client(url="https://api.example.com/updates", callback=process_response)
   def fetch_updates():
       pass

