Configuration
=============

You can configure `dequest` globally using `DequestConfig`.

Cache Provider
--------------

.. code-block:: python

   from dequest import DequestConfig

   DequestConfig.CACHE_PROVIDER = "redis"  # Options: "in_memory", "redis" and "django"

Redis Configuration
-------------------
If you set "redis" as cache provider, you should set the redis server configuration also.

.. code-block:: python

   from dequest import DequestConfig

   DequestConfig.config(
      cache_provider="redis", # defaults to "in_memory"
      redis_host="my-redis-server.com",
      redis_port=6380,
      redis_db=1,
      redis_password="securepassword",
      redis_ssl=True,
   )

Calling the `.config()` method is optional. You can use it during application initialization if you need to specify custom configurations instead of the defaults.