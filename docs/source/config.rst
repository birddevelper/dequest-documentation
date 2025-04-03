Configuration
=============

You can configure `dequest` globally using `DequestConfig`.

Cache Provider
--------------

.. code-block:: python

   from dequest.config import DequestConfig

   DequestConfig.CACHE_PROVIDER = "redis"  # Options: "in_memory", "redis" and "django"

Redis Configuration
-------------------
If you set "redis" as cache provider, you should set the redis server configuration also.
.. code-block:: python

   DequestConfig.REDIS_HOST = "localhost"
   DequestConfig.REDIS_PORT = 6379
   DequestConfig.REDIS_PASSWORD = None
   DequestConfig.REDIS_SSL = False
