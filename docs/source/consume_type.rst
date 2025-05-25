Response Consuming Types
=========================

APIs can return responses in different formats, such as JSON, XML, or plain text. The `consume` parameter in `dequest` allows you to specify how the response should be parsed before it is deserialized into the provided `dto_class`.

This is configured using the `ConsumerType` enum.

ConsumerType Enum
-----------------

The `ConsumerType` enum includes the following options:

- `ConsumerType.JSON` – Parses the response as JSON (default).
- `ConsumerType.XML` – Parses the response as XML.
- `ConsumerType.TEXT` – Reads the response as plain text.

Using the `consume` Parameter
-----------------------------

You can pass the `consume` argument to the `sync_client` or `async_client` decorator to control how the response is handled.

**JSON (default)**

If the API returns JSON, no need to set `consume` explicitly:

.. code-block:: python

   from dequest import sync_client

   @sync_client(
       url="https://api.example.com/users/{user_id}",
       dto_class=UserDTO,
   )
   def get_user(user_id: int):
       pass

**XML**

To consume an XML response, set `consume=ConsumerType.XML`:

.. code-block:: python

   from dequest import sync_client, ConsumerType

   @sync_client(
       url="https://api.example.com/users/{user_id}",
       dto_class=UserDTO,
       consume=ConsumerType.XML,
   )
   def get_user(user_id: int):
       pass

**TEXT**

For plain text responses (e.g. messages, logs, or simple strings), use `ConsumerType.TEXT`:

.. code-block:: python

   from dequest import sync_client, ConsumerType

   @sync_client(
       url="https://api.example.com/message",
       dto_class=MessageDTO,
       consume=ConsumerType.TEXT,
   )
   def get_message():
       pass


Note that when using `ConsumerType.TEXT`, the response will not be deserialized into a DTO class. Instead, it will return the raw text content.