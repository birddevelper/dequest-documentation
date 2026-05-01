DTO Mapping
===========

`dequest` supports automatic response mapping into custom DTO classes using the `dto_class` parameter. it's supported by all client decorators: `@sync_client`, `@async_client`, and `@async_await_client`.

When `consume=ConsumerType.JSON`, the JSON response is deserialized and mapped into the provided DTO class.
When `consume=ConsumerType.XML`, the XML response is parsed and mapped in the same way.

Assuming we have the following JSON response from the API:

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

.. code-block:: python

from dataclasses import dataclass

   @dataclass 
   class ItemDTO:
       id: int
       name: str

   @sync_client(
       url="https://api.example.com/items/{item_id}",
       dto_class=ItemDTO,
       consume=ConsumerType.JSON, # Optional since JSON is the default
       source_field="data.item",
   )
   def get_item(item_id: int):
       pass

Only the selected `source_field` is mapped into `ItemDTO`.

Nested DTOs are supported automatically:

.. code-block:: python

from dataclasses import dataclass

   @dataclass
   class ProfileDTO:
       id: int
       email: str

   @dataclass
   class UserDTO:
       id: int
       name: str
       profile: ProfileDTO


   @async_await_client(
       url="https://api.example.com/users/{user_id}",
       dto_class=UserDTO,
       source_field="data.user",
   )
   async def get_user(user_id: int):
       pass

`dequest` will create a `ProfileDTO` object inside the mapped `UserDTO`.

Lists of DTOs are also supported when the selected source field resolves to an array:

.. code-block:: json

   {
       "status": "ok",
       "data": {
           "items": [
               {"id": 1, "name": "Item 1"},
               {"id": 2, "name": "Item 2"}
           ]
       }
   }

No need to change the DTO definition:

.. code-block:: python

   @async_client(
       url="https://api.example.com/items",
       dto_class=ItemDTO,
       consume=ConsumerType.JSON,
       source_field="data.items",
   )
   def list_items():
       pass

The `list_items` function returns a list of `ItemDTO` objects.

Important notes:

- `dto_class` cannot be used with `ConsumerType.TEXT`.
- `source_field` is optional; if omitted, `dequest` attempts to map the entire response into the DTO.
- This mapping behavior is identical across `@sync_client`, `@async_client`, and `@async_await_client`.
