simple-file-storage-with-HTTP-API
===============

File storage with HTTP access

This is an implementation of a simple http-server that allows you to upload, download and delete files using the HTTP API.
The server stores only unique files. The uniqueness of a file is determined by the value of SHA256 hash function where the function argument is a file.

It is tested with Linux Mint 19, python 3.6.6

NOTE: The server doesn't handle empty request body. If it happens then the server freezes.  It relates BaseHTTPRequestHandler. When rfile starts reading that it opens stream and wait data from client. If data does't come that server will be awaiting data.

API description
---

### 1. Upload file

Sending an upload request:
* Use POST-request with URI: “/v1/files/” .
* Add request headers. "Content-Length" is required request header (Set to the number of bytes you are uploading).
* Add	a request body. A request body is required.
* Send a request.

Request requirements
Handling only requests which have single-resource bodies. 	
API doesn't support multipart content.
A file is any kind of data contained in a request body.


Example: Sending an upload request

    POST http://127.0.0.1:8080 HTTP/1.1
    Content-Length: [NUMBER_OF_BYTES_IN_FILE]

    [FILE_DATA]


Example: a server response
If the request succeeds, the server returns the HTTP 200 OK status code and SHA256
hash of an uploaded file. A hash looks like the string which consists of hexadecimal digits. SHA256_HASH is a unique identifier of a file in the file system of storage.

    HTTP/1.1 200
    Content-Type: application/octet-stream
    Content-Length: [NUMBER_OF_BYTES_IN_FILE]

    [SHA256_HASH]


#### Status code

 200 - This file already exists
 
 201 - File created
 
 400 - Bad request


### 2. Download file

Sending a download request:
* Use HTTP-request GET with URI: "/[sharedirname]/filepath"
where sharedirname is the name of the shared dir and the filepath is the path of the file you want to download. 
* Send a request.


Example: Sending a downloaded request

    GET /v1/files/[SHA256_HASH] HTTP/1.1


Example: a server response
If the request succeeds, the server returns the HTTP 200 OK status code and a file.

    HTTP/1.1 200
    Content-Type: application/octet-stream
    Content-Length: [NUMBER_OF_BYTES_IN_FILE]

    [FILE_DATA]


#### Status code

  200 - Successful request
  
  400 - Bad request
  
  404 - Not found


### 3. Delete file

Sending a delete request:
* Use HTTP-request DELETE with URI: “/v1/files/SHA256_HASH
where SHA256_HASH is the hash of deleted file. A hash looks like a string which consists of hexadecimal digits. SHA256_HASH is a unique identifier of a file in the file system of storage
* Send a request


Example: a server response

    DELETE /v1/files/[SHA256_HASH] HTTP/1.1


Example: a server response
If the request succeeds the server returns the HTTP 200 OK status code.

    HTTP/1.1 200


#### Status code

  200 - Successful request
  
  400 - Bad request
  
  404 - Not found
  


