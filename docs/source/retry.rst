Retry
=================

If an API request fails due to network issues or any server errors, `dequest` can automatically retry the request. This helps improve resiliency, especially when working with flaky or unstable network conditions or APIs.

Basic Retry
-----------

You can enable retries by setting the `retries`, `retry_on_exceptions` and `retry_delay` parameters in the client decorator. Here's an example using the `sync_client`:

.. code-block:: python

   from dequest import sync_client

   @sync_client(url="https://api.example.com/data", retries=3, retry_delay=2, retry_on_exceptions=(ConnectionError, TimeoutError))
   def get_data():
       pass

This will retry the request up to 3 times with a 2-second delay between each attempt if the call fails due to a `ConnectionError` or `TimeoutError`. If the request succeeds, it returns the response data as usual.


- **`giveup`**: A callable that determines whether to stop retrying based on the raised exception.

Here's an example using `async_client` with more sophisticated retry logic:

.. code-block:: python

   from dequest import async_client, HttpMethod
   from requests import HTTPError
   import http


   @async_client(
       dto_class=UserDTO,
       url="https://dummyjson.com/auth/me",
       method=HttpMethod.GET,
       retry_on_exceptions=(HTTPError,),
       retries=3,
       retry_delay=1,
       # Retry only if the error is an internal server error (500)
       giveup=lambda e: e.response.status_code != http.HTTPStatus.INTERNAL_SERVER_ERROR,
   )
   def get_current_user() -> UserDTO:
       """
       Function to get the current user.
       This function retrieves the current user's information from the API and returns it as a UserDTO object.
       """


- **`retry_delay`**: Delay in seconds between retries. Can be a static value or a function returning iterator.

Here is an example of using a function to create a backoff strategy:

.. code-block:: python
    
   from dequest import async_client, HttpMethod
   import time

   def backoff_strategy():
       delay = 1
       while True:
           yield delay
           delay *= 2  # Exponential backoff

   @async_client(
       url="https://api.example.com/resource",
       method=HttpMethod.GET,
       retries=5,
       retry_delay=backoff_strategy,
       retry_on_exceptions=(ConnectionError, TimeoutError)
   )
   async def fetch_resource():
       pass