# curl command

Curl stands for “Client for URLs”.

`curl -u user:pass http://localhost:8080/hello`  
The `-u` (or `--user`) option in `curl` supplies **HTTP Basic Authentication** credentials.  
It tells `curl` to include an `Authorization: Basic` header in the request.

Behind the  scenes,  cURL  encodes  the entire string `<username>:<password>`  in  Base64  and sends  it  as  the  value  of  the `Authorization:` header prefixed with the string `Basic`.
