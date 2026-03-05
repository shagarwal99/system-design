## Design URL Shortner (Like Bit.ly)

### Functional Requirements

#### Core requirements
- Create a short URL for a given long URL
- Redirect short URL to original URL
- Track statistics (clicks, redirects, etc.)
- Handle URL expiration (optional)
- Support custom short codes (optional)

#### Optional requirements
- Authentication and user management
- Analytics (clicks, redirects, etc.)

### Non-Functional Requirements
- The system should ensure uniqueness for every short url i.e every new URL should point to  just one URL
- The redirection should occur with minimal delay
- The system should be available 99.9999% percent of the time (Here availability > consistency)
- The system should handle a huge amount of requests and URL


#### High Level Design

### Users should be able to submit a long URL and get a short URL
<div style="text-align: center;">
   <img src="../images/bitly-1.png">
</div>
<br>

- The user will send a POST request to the server with the original URL, alias and expiration time. After this, the server will
  - Validate the long form URL if it is a valid URL. If the original url is valid, We can ask if the URL provided is already shortened, what will be the next step (generate new short url or return the existing one)
  - If the URL is not shortened, generate a short code for the URL using some function and store the mapping in the database.
  Also, if user has provided an alias, we can use that as the short code. But we need to check if the alias is already taken.
  - Return the short URL to the user


### Users should be able to access the original URL using the short URL

The following process will happen
 - The users browser will send a GET request to the server with the short URL
 - The server will look up the short URL in the database.
 - If the short URL is there in the database and not expired, the server will return the original URL
 - If the short URL is not there in the database return 404 status. Return 401 gone status if the short URL is expired
 - The server will return back the original URL instructing the browser to redirect to it

 Now, we can use two types of HTTP redirects
 - 301 Moved Permanently - The URL has been permanently moved to a new location. Browsers typically cache this response, meaning subsequent requests for the same short URL might go directly to the long URL, bypassing our server.
 - 302 Found - The URL has been temporarily moved to a new location. Browsers do not cache this response, ensuring that future requests for the short URL will always go through our server first.

 The 302 redirect is preferred for URL shortening services because it allows the service to track clicks and analytics without the complexity of managing cache invalidation.


### Deepdives
- #### How to always generate unique short codes?
    There are multiple approaches to it. A simple version can be to get machine id and maintain an incremental counter. These two can be passed to the hash function to generate a unique short code. If the resulting string is long, we can truncate it to a desired length.
    We also need to enforce a unique constraint on the short code in the database to handle collisions. We can also use a datetime value , combine a unique machine id with microsecond or nanosecond epoch and append 2 to 4 random bytes for very low collision risk.

- #### How do we make sure redirects are fast?
    - We can create index on the short URL column in the database to make the lookup faster.
    - The main optimization will be to use caching using Redis or similar in-memory data store to cache the short URL to original URL mapping. When the request comes in, the server first checks in the cache if the short url is present or not. If it is not there, it will go and read from the database.