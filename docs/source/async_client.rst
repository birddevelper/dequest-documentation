Using `async_client`
====================

The `@async_client` decorator lets you make non-blocking HTTP requests.

.. code-block:: python

   from dequest import async_client

   @async_client(url="https://api.example.com/notify", method="POST")
   def notify():
       pass

   notify()  # Fire-and-forget call


Using Callbacks
---------------

If you need to handle the response asynchronously, use a callback function. Callbacks let you process API responses asynchronously.

.. code-block:: python

   async def process_response(data):
       print("Received:", data)

   @async_client(url="https://api.example.com/updates", callback=process_response)
   def fetch_updates():
       pass
