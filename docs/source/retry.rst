Automatic Retries
=================

If an API request fails due to network issues or server errors, `dequest` can automatically retry the request.

.. code-block:: python

   from dequest import sync_client

   @sync_client(url="https://api.example.com/data", retries=3, retry_delay=2)
   def get_data():
       pass

This will retry up to 3 times with a 2-second delay between attempts.
