Source Field
=================

**Using `source_field`**

`source_field` lets you map a nested field from a JSON response instead of the full payload.
This is useful when the API returns metadata around the actual data you need.

.. code-block:: python

   from dequest import async_await_client, ConsumerType

   @async_await_client(
       url="https://api.example.com/items/{item_id}",
       method="GET",
       consume=ConsumerType.JSON,
       source_field="data.item",
   )
   async def get_item(item_id: int):
       pass

   async def main():
       item = await get_item(item_id=42)
       print(item)

In this example, if the API response is:

.. code-block:: json

   {
       "status": "ok",
       "data": {
           "item": {
               "id": 42,
               "name": "Widget",
           }
       }
   }

then the decorated function returns only the value at `data.item`.