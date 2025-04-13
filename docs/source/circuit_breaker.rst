Circuit Breaker
===============

Basic Usage
-----------
To use a circuit breaker with `dequest`, create an instance of `CircuitBreaker` and pass it to the `sync_client` or `async_client` decorator.

.. code-block:: python

   from dequest import sync_client
   from dequest import CircuitBreaker

   # Define a Circuit Breaker with failure threshold of 3 and recovery timeout of 10 seconds
   breaker = CircuitBreaker(failure_threshold=3, recovery_timeout=10)

   @sync_client(url="https://api.unstable.com/data", circuit_breaker=breaker)
   def fetch_data():
       pass

If the API fails **3 times in a row**, the circuit breaker opens and blocks further requests for **10 seconds**.
Note The retry logic wraps the entire operation so that only if all attempts fail, then the circuit breaker record one failure:

.. code-block:: python

   from dequest import sync_client
   from dequest import CircuitBreaker

   breaker = CircuitBreaker(failure_threshold=3, recovery_timeout=10)

   @sync_client(url="https://api.unstable.com/data", circuit_breaker=breaker, retries=2)
   def fetch_data():
       pass

In the above example if API fails 2 * 3 times, the circuit opens and blocks requests for 10 seconds.


Handling Circuit Breaker Open Errors
------------------------------------
When the circuit breaker is open, calling the function will raise a `CircuitBreakerOpenError`:

.. code-block:: python

   from dequest import CircuitBreakerOpenError

   try:
       response = fetch_data()
   except CircuitBreakerOpenError:
       print("Circuit breaker is open! Too many failures, try again later.")

Using a `fallback_function`
---------------------------

Instead of raising an error when the circuit breaker is open, you can define a **fallback function** that provides an alternative response. 

For example, if the main API fails, we can return a cached response or default data:

.. code-block:: python

   from dequest import sync_client
   from dequest import CircuitBreaker

   # Define a fallback function
   def fallback_response():
       return {"message": "Service unavailable, returning cached data instead"}

   # Create a Circuit Breaker with a fallback function
   breaker = CircuitBreaker(
       failure_threshold=3,
       recovery_timeout=10,
       fallback_function=fallback_response  # Fallback instead of raising an error
   )

   @sync_client(url="https://api.unstable.com/data", circuit_breaker=breaker)
   def fetch_unstable_data():
       pass

   # Simulating multiple failed requests
   for _ in range(5):  # Exceeds the failure threshold
       response = fetch_unstable_data()
       print(response)  # This will return the fallback response instead of failing

This helps build a **resilient, fault-tolerant** system that gracefully handles API failures.
