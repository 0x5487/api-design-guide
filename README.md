# Api Design Guide


### Authentication
A RESTful API should be stateless. This means that request authentication should not depend on cookies or sessions. Instead, each request should come with some sort authentication credentials.
We will put our token into header

```
Authorization: XXXXXXXXXXX
```

### Use UTC times formatted in ISO8601
Accept and return times in UTC only. Render times in ISO8601 format, e.g.:
```json
"created_at": "2012-01-01T12:00:00Z"
```
### Use HTTP status codes
The HTTP standard provides over 70 status codes to describe the return values. We don't need them all, but  there should be used at least a mount of 10.

200 – OK – Eyerything is working
201 – OK – New resource has been created
204 – OK – The resource was successfully deleted

304 – Not Modified – The client can use cached data

400 – Bad Request – The request was invalid or cannot be served. The exact error should be explained in the error payload. E.g. „The JSON is not valid「
401 – Unauthorized – The request requires an user authentication
403 – Forbidden – The server understood the request, but is refusing it or the access is not allowed.
404 – Not found – There is no resource behind the URI.
422 – Unprocessable Entity – Should be used if the server cannot process the enitity, e.g. if an image cannot be formatted or mandatory fields are missing in the payload.

500 – Internal Server Error – API developers should avoid this error. If an error occurs in the global catch blog, the stracktrace should be logged and not returned as response.

### Version your API
the URL has a major version number (v1), but the API has date based sub-versions which can be chosen using a custom HTTP request header. In this case, the major version provides structural 
stability of the API as a whole while the sub-versions accounts for smaller changes (field deprecations, endpoint changes, etc).Make the API Version mandatory and do not release an unversioned API. 
Use a simple ordinal number and avoid dot notation such as 2.5. We are using the url for the API versioning starting with the letter

`v1/data/xxx`

Please also send the subversion via header.  We use date format.<br/>
`version: 20160228`

### Use HTTP headers for serialization formats
Both, client and server, need to know which format is used for the communication. The format has to be specified in the HTTP-Header.

Content-Type defines the request format.
Accept defines a list of acceptable response formats.

### Use snake_case for field names
we use underline to split words.

```json
{
    "service_class": "first"
}
```

### Don't use an envelope by default, but make it possible when needed
normal example
```json
{
  "Success": true,
  "Code": "1.0",
  "Message": null,
  "Result": {
      "name" : "mike"
  }
}
```
better approach

```json
{
    "name" : "mike"
}
```

### Provide standard timestamps
Provide created_at and updated_at timestamps for resources by default, e.g:

```json
{
  // ...
  "created_at": "2012-01-01T12:00:00Z",
  "updated_at": "2012-01-01T13:00:00Z",
  // ...
}
```

### Generate structured errors
A JSON error body should provide a few things for the developer - a useful error message, a unique error code (that can be looked up for more details in the docs) and possibly a detailed description. 

```json
{
  "error_code" : "invalid_input",
  "message" : "Something bad happened",
  "details":[]
}
```



