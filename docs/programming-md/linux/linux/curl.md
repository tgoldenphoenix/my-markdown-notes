# `curl`

Curl stands for “Client for URLs”.

`curl -u user:pass http://localhost:8080/hello`  
The `-u` (or `--user`) option in `curl` supplies **HTTP Basic Authentication** credentials.  
It tells `curl` to include an `Authorization: Basic` header in the request.

Behind the  scenes,  cURL  encodes  the entire string `<username>:<password>`  in  Base64  and sends  it  as  the  value  of  the `Authorization:` header prefixed with the string `Basic`.

---

`curl` stands for "client url"

On MacOS, curl comes pre-installed.

You can use curl with a REST-api.

```
$ curl "https:/ /awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
$ unzip awscliv2.zip
$ sudo ./aws/install
```

The `-o` (output flag) instructs `curl` to save the fetched data (instead of printing it to the terminal).

## wget

- `wget` is optimized for simple, robust file downloading (especially recursive and background downloads).
- `curl` is optimized for data transfer and protocol flexibility, making it the primary tool for API interaction and scripting.

`wget` saves output to a file by default, while `curl` Prints to standard output (stdout) by default.