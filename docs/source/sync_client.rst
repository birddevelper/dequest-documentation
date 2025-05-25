Using `sync_client`
===================

The `@sync_client` decorator allows you to make synchronous HTTP requests declaratively.

**Basic Example**

.. code-block:: python

    from dequest import sync_client, QueryParameter
    from typing import List
    from dataclasses import dataclass

    @dataclass
    class UserDto:
        id: int
        name: str
        city: str

        def __init__(self, id, name, city):
            self.id = id
            self.name = name
            self.city = city

    @sync_client(url="https://jsonplaceholder.typicode.com/users", dto_class=UserDto)
    def get_users(city: QueryParameter[str, "city_name"]) -> List[UserDto]:
        pass

    users = get_users(city="New York")
    print(users)


This sends a GET request and automatically handles response parsing. The `dto_class` parameter specifies the data class to map the JSON response to.

**Parameters**

**Parameters**

- **url**: URL template with placeholders for path parameters.
- **dto_class**: DTO class to map response data.
- **source_field**: Source field to use for mapping response data. Leave None to map the whole response.
- **method**: HTTP method (GET, POST, PUT, DELETE).
- **timeout**: Request timeout in seconds.
- **retries**: Number of retries on failure.
- **retry_on_exceptions**: Exceptions to retry on.
- **retry_delay**: Delay in seconds between retries. Can be a static value or a function returning an iterator.
- **giveup**: Function to determine if a retry should be given up.
- **auth_token**: Optional Bearer Token (static string or function returning a string).
- **api_key**: Optional API key (static string or function returning a string).
- **headers**: Optional default headers (can be a dict or a function returning a dict).
- **enable_cache**: Whether to cache GET responses.
- **cache_ttl**: Cache expiration time in seconds.
- **circuit_breaker**: Instance of CircuitBreaker (optional).
- **consume**: The type of data to consume (JSON, XML, TEXT).

This decorator supports various features such as authentication (both static and dynamic), retries, logging, query parameters, form parameters, timeout management, circuit breaker functionality, and caching.

