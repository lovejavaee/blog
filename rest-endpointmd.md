# Allowed request methods
- POST
create a resource with a new ID

- PUT
update a resource with existing ID

- PATCH
partially update a resource (of record with existing id) fields not provided in the input will remain unchanged.

- GET
retrieve a resource by specifying the with existing id in the request URI. the get method does not allow a request body.
For GET requests with timestamp, we return records where start timestamp >=today's date

- DELETE
remove a resource with existing id

# Allowed response codes

- 200 SUCCESS
request handles successfully

- 201 CREATED
new record created successfully

- 404 NOT FOUND
record not found

- 400 BAD REQ
client-side error

- 401 UNAUTH
unauthorised request

- 405 METHOD NOT ALLOWED
request method is not allowed for the target API

- 500 SYSERROR
server side error detected

- 409 CONFLICT
conflict of data


