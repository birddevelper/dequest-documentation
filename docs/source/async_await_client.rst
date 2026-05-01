Using `async_await_client`
==========================

The `@async_await_client` decorator makes asynchronous HTTP requests and returns a result.
The decorated function must be awaited inside an async function.


.. code-block:: python

   from dequest import async_await_client, ConsumerType

   @async_await_client(
       url="https://api.example.com/items/{item_id}",
       method="GET",
   )
   async def get_item(item_id: int):
       pass

   async def main():
       item = await get_item(item_id=42)
       print(item)


`async_await_client` behaves like a normal async function call and returns the request result.
It is the async/await equivalent of `@async_client`, which is designed for fire-and-forget calls.

- **url**: URL template with placeholders for path parameters.
- **dto_class**: The DTO class to map the response data.
- **source_field**: Source field to use for mapping response data. Leave None to map the whole response.
- **method**: HTTP method (GET, POST, PUT, DELETE).
- **timeout**: Request timeout in seconds.
- **retries**: Number of retries on failure.
- **retry_on_exceptions**: Exceptions to retry on.
- **retry_delay**: Delay in seconds between retries, or a callable returning an iterator.
- **giveup**: Function to determine if the retry loop should stop.
- **auth_token**: Optional Bearer token string or callable returning a string.
- **api_key**: Optional API key string or callable returning a string.
- **headers**: Optional default headers dict or callable returning a dict.
- **enable_cache**: Whether to cache GET responses.
- **cache_ttl**: Cache expiration time in seconds.
- **circuit_breaker**: Optional `CircuitBreaker` instance.
- **callback**: Optional function to process the response.
- **consume**: Type of data to consume. `ConsumerType.JSON`, `ConsumerType.XML`, or `ConsumerType.TEXT`. Default is `ConsumerType.JSON`. If `consume` is `ConsumerType.TEXT`, `dto_class` cannot be used.

**Example with callback**

.. code-block:: python

   async def process_response(data):
       print("Response received:", data)

   @async_await_client(
       url="https://api.example.com/updates",
       callback=process_response,
   )
   async def fetch_updates():
       pass

   async def main():
       result = await fetch_updates()
       print("Returned value:", result)


