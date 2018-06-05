#REST

##From [Understanding](https://spring.io/understanding/REST) REST:

REST (Representational State Transfer) was introduced and defined in 2000 by Roy Fielding in his doctoral dissertation. REST is an architectural style for designing distributed systems. It is not a standard but a set of constraints, such as being stateless, having a client/server relationship, and a uniform interface.

*Rationale*
Since we primarily develop web-based APIs, we would benefit from REST, which was designed for the web
Understanding the Richardson Maturity Model is important for why we strive for REST APIs - or perhaps it helps understand the problems in not following REST. 
From Martin Fowler's article on the Richardson Maturity Model:
http://martinfowler.com/articles/images/richardsonMaturityModel/overview.png

The minimum maturity level for our APIs is level 2.
Design your APIs in terms of the resources it represents.

Additional reading:
- [REST in Practice](https://www.safaribooksonline.com/library/view/rest-in-practice/9781449383312/ch01s05.html)
- [REST CookBook](http://restcookbook.com/Miscellaneous/richardsonmaturitymodel/)

Terminology
There is a lot of terminology used in this document. Usually links will be provided to a source that defines them if they are not defined here.

To begin with, the following terms are defined as they will be used in this documentation because they may have different meanings when used elsewhere.

API
An API is a Web API product. It doesn't mean each Endpoint or Library.

Endpoint
An endpoint is a URI to access each functionally in the API. An endpoint can provide multiple functionality by switching the HTTP Method.

HTTP
REST is not strictly related to HTTP, but it is most commonly associated with it.

 Expose the API over HTTP so that it may be consumed easily in a web environment.

 *Rationale*
HTTP is defined by internet standards. This means that various devices, tools, and libraries understand the meaning of its methods and status codes out-of-the-box and they may alter their behavior accordingly. We can take advantage of this, but only if we use HTTP following its convention.

Resource naming / URIs
A resource should be named as a plural noun that makes the meaning clear. In conjunction with HTTP methods, this makes the usage of a URI apparent.

The URI is constructed based on the of the resource name.

For example, an Address resource will be exposed under the /addresses endpoint.

See http://www.restapitutorial.com/lessons/restfulresourcenaming.html

Search/query methods
There may be a need to search for resources - find them by something other than their ID. The methods should be provided under a /search path. A GET /search request may return the different available methods. Since the combination of allowed/required parameters may vary by search, this separation makes it more clear and user-friendly (also easier to implement).

For example, if we wanted to offer a findByZipcode search of Address resources, an example of a corresponding search URI would be /addresses/search/findByZipcode?zipcode=2130004 and it would return a collection of Address resources with matching zipcode.

Versioning
The version of the API is included in the URI. Only the major version is included, since according to Semantic Versioning it is the only version that has backwards incompatible changes and thus must be exposed as a different URI.

For example, the URI for the Address resource from version 1 of the API would be /v1/addresses.


Option 2 - content negotiated version
Using the HTTP Accept header (or perhaps the Accept-Version header), clients can request different versions (representations) by media type. Instead of application/json the media type could be application/vnd.xxx.travel.address-v1+json

References

https://www.troyhunt.com/your-api-versioning-is-wrong-which-is/
http://www.lexicalscope.com/blog/2012/03/12/how-are-rest-apis-versioned/
http://stackoverflow.com/questions/10742594/versioning-rest-api
http://stackoverflow.com/questions/389169/best-practices-for-api-versioning

HTTP methods
When interacting with resources, the HTTP method describes the logical action being performed on that resource.

Depending on the requirements for the resource, an API should offer all or some of the following methods.

GET

Retrieve a resource

GET /addresses/1
GET /addresses
 

POST

Create a *new* resource

POST /addresses
SHOULD return Location of the resource created.

MAY return a representation of the resource created in the body.

PATCH

Update an existing resource, including partial updates

PATCH /addresses/1
Useful when there are many fields you do NOT want to update.
PUT Create or replace the existing resource with ID PUT /addresses/1    Useful when the client knows the ID to be created.
DELETE

Delete an existing resource

DELETE /addresses/1
Not necessarily a physical delete
In addition to the above most common HTTP methods, the following also exist which may be useful in certain situations.

HEAD    Retrieve just the headers (no body) 
HEAD /addresses/1
OPTIONS Check options available for a resource or server    
 
For reference, most of the HTTP methods were originally defined in RFC2616.


HTTP status codes
Use the HTTP status code as an indication of the result of the HTTP request.

Status code series
From the Hypertext Transfer Protocol (HTTP) Status Code Registry, the defined status code series are:

1xx: Informational - Request received, continuing process
2xx: Success - The action was successfully received, understood, and accepted
3xx: Redirection - Further action must be taken in order to complete the request
4xx: Client Error - The request contains bad syntax or cannot be fulfilled
5xx: Server Error - The server failed to fulfill an apparently valid request
Within each series there are specific status codes with more fine-grained meaning. Based on the semantics of the response, use an appropriate status code.

The "flavor" text is provided here as a guide, but the numerical code is the only guaranteed value in the HTTP header.

The usage example is just one typical example and NOT meant to be an exhaustive list of use cases.

Success status codes
200 OK  
This is the most generic success code.

It provides the least context and therefore should be used when none of the more specific codes apply.

Most typically in response to a GET request.
201 Created 
A new resource has been successfully created.

A Location header should give the URI of the created resource.

POST or PUT request to create a new resource
202 Accepted    
The request has been accepted for processing, but the processing is not complete.

The response may include useful information about how to check the status of the processing.

The request is to be processed asynchronously or later by a batch
204 No Content  The request has been fulfilled and there is no content to return (in the body). DELETE or save changes (PUT/PATCH) to a resource from a UI but leave user's view the same.
Redirection

301 Moved Permanently   
The requested resource has permanently moved and can be found at the new URI.

 
302 Found   The client should continue to use the effective request URI because the server may change the redirection in the future.     
3xx     The server SHOULD generate a Location header field in the response containing a URI reference for the different URI. The user agent MAY use the Location field value for automatic redirection.      
Failure status codes
Client error
400 Bad Request The client has sent an invalid request that cannot or should not be processed.  Request validation fails
401 Unauthorized    
Authentication failed - credentials are missing or incorrect.

WWW-Authenticate header MUST be returned.

Not logged in user tries to access a protected resource
403 Forbidden   The requesting user does not have authorization to access the resource. Regular user tries to access resources restricted to admins
404 Not Found   The requested resource does not currently (logically) exist OR the server is not willing to disclose that it exists.    A resource is requested that hasn't been created yet
405 Method Not Allowed  
The HTTP method is not currently allowed on the resource.

Allow header with currently supported methods MUST be returned.

Attempt to update (PUT) an unmodifiable resource
406 Not Acceptable  The server cannot respond with a representation that is acceptable according to the client's sent Accept headers. See RFC7231 sec 5.3. Content Negotiation  The client only accepts JSON but the server only produces XML
409 Conflict    
The target resource state is in conflict with the action requested.

The server SHOULD return a payload that includes enough information for a user to recognize the source of the conflict.

 

Request to update a field in a way that no longer makes sense (due to a third-party update)
410 Gone    The requested resource is not available and it is known that it never will be available.    It is past the end date of a limited time resource.
411 Length Required The client has failed to specify the Content-Length header and the server will not accept the request without it.    
412 Precondition Failed Conditions set by the client have not been met. Client uses If-Match header with an old ETag
415 Unsupported Media Type  The payload sent by the client is not in a format supported by the server. Usually this is determined by Content-Type or Content-Encoding   Client sends XML but server only handles JSON
428 Precondition Required   No condition was sent with the request but the server requires a condition such as If-Match.    Server requires a client to send an ETag in the If-Match header when updating, but it is not present.
429 Too Many Requests   The client has sent too many requests in a given amount of time and are being denied (temporarily, usually). A Retry-After header may be included to indicate when the client can try again.    Rate limiting access to an API to prevent DoS
Server error
500 Internal Server Error   
An unexpected error occurred during processing the request and it cannot be fulfilled.

Data store error
501 Not Implemented The endpoint or requested functionality is not implemented at this time.    An endpoint is planned but not implemented yet.
502 Bad Gateway While processing the request, an upstream service returned an error.    A dependent service returns 500.
503 Service Unavailable The service is down. Retry again later. Typically returned by a proxy when the service is not up.
504 Gateway Timeout An upstream service timed out while trying to complete the request. An outbound HTTP call times out.
References
http://racksburg.com/choosing-an-http-status-code/

https://httpstatuses.com/

https://gist.github.com/vkostyukov/32c84c0c01789425c29a

https://github.com/for-GET/http-decision-diagram

Content negotiation
The Accept header is sent with the HTTP request by the client indicating the media types it can handle (optionally indicating preference among multiple types). The service will return a 406 (not acceptable) status if the Accept header contains only unsupported media types. Lack of an Accept header means any media type may be returned.

The Content-Type header indicates the media type that is contained in the body (or would be in case of HEAD). A request or response may contain this header. It is used to determine how to interpret the included body.

These headers are how clients and APIs communicate about media types. Depending on the specifications, an API may need to support multiple media types, though the most common will be JSON and/or XML.

References
The following references are good resources on REST (pun intended):

[REST in Practice](https://www.safaribooksonline.com/library/view/rest-in-practice/9781449383312/)

