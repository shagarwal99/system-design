## Caching

It is a concept of storing frequently accessed data in a location that is easily and quickly accessible. The purpose of caching is to reduce the time and enhance the efficiency of a system. A cache can be maintained at different places:

- ### External Caching

  An external cache is a standalone cache service that your application talks to over the network. You store frequently accessed data in something like **Redis** or **Memcached** so you do not have to hit the database every time. External caches scale well because every application server can share the same cache. They also support eviction policies like **LRU** and **expiration via TTL** so your memory footprint stays controlled.
- ### Content Delivery Network (CDN)

  A CDN is a geographically distributed network of servers that caches content close to users. Instead of every request traveling to your origin server, a CDN stores copies of your static content at edge servers around the world. So when a user requests an image from the app, the request will then go to the nearest CDN and will try to look there. If the asked resource is cached there, it will be served from that server and if not, it will ask the origin server and serve the asked image
- ### Client Side Caching

  Client-side caching stores data close to the requester to avoid unnecessary network calls. This usually means the user's device, like a browser (HTTP cache, localStorage) or mobile app using local memory or on-device storage. For user-facing caching, you have limited control from the backend. Data can go stale and invalidation is harder. The Strava app keeps your run data on the device while you are offline and syncs it later. A browser reusing a previously downloaded image from disk is also caching.
- ### In-Process Caching

  As hardware of the server improves, we can use the memory to cache data directly inside the application process instead of always calling out to Redis or the database. Reads from local memory are even faster than reads from Redis because they avoid any network call. This type of caching makes sense for the values like:
    - Configuration values
    - Feature Flags
    - Precomputed values

  In-process caching is blazing fast, but it comes with obvious limitations. Each instance of your application has its own cache, so cached data is not shared across servers. If one instance updates or invalidates a cached value, the others will not know. In short, this is only good for values that rarely change

## Cache Architectures

- ### Cache Aside(Lazy Loading): 

  The most common cache pattern. The application checks the cache first for the requested data. If it is found there it is served directly otherwise,it queries the source to fetch the requested information. The entry is then later added to the cache for subsequent lookups
- ### Write-through:

  The application uses the cache as the main data store, reading and writing data to it, while the cache is responsible for reading and writing to the database.
    - The application adds/updates the entry in cache
    - The cache synchronously writes entry into the database
    - Return

  Write-through is a slow overall operation due to the write operation, but subsequent reads of just written data are fast. Users are generally more tolerant of latency when updating data than reading data. Data in the cache is not stale. 

  Disadvantages:

- Data present in the cache may never be read
- In case of an empty node(due to failure) the node will not cache entries until the entry is updated in the database. Cache-aside in conjunction with write through can mitigate this issue.
- ### Write-behind

  The write into the database from the cache is done asynchronously.



  Disadvantages:

  - There could be data loss if the cache goes down prior to its contents hitting the data store.
  - It is more complex to implement write-behind than it is to implement cache-aside or write-through.
- ### Refresh-ahead

  You can configure the cache to automatically refresh any recently accessed cache entry prior to its expiration. Refresh-ahead can result in reduced latency vs read-through if the cache can accurately predict which items are likely to be needed in the future.



  Disadvantages:

  Need to maintain consistency between caches and the source of truth such as the database through [cache-invalidation](https://en.wikipedia.org/wiki/Cache_replacement_policies) which in itself is a highly complex problem to solve