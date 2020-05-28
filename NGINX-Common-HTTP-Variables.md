# Below are some of the HTTP variables 

### Example of HTTP Variable e.g. $scheme variable 

```
location ~ /adminfoo(.*)$ {
    return 301 $scheme://$http_host/admin$1;
}
```
```
$request_id
unique request identifier generated from 16 random bytes, in hexadecimal (1.11.0)
$request_length
request length (including request line, header, and request body) (1.3.12, 1.2.7)
$request_method
request method, usually “GET” or “POST”
$request_time
request processing time in seconds with a milliseconds resolution (1.3.9, 1.2.6); time elapsed since the first bytes were read from the client
$request_uri
full original request URI (with arguments)
$scheme
request scheme, “http” or “https”
$sent_http_name
arbitrary response header field; the last part of a variable name is the field name converted to lower case with dashes replaced by underscores
$sent_trailer_name
arbitrary field sent at the end of the response (1.13.2); the last part of a variable name is the field name converted to lower case with dashes replaced by underscores
$server_addr
an address of the server which accepted a request
Computing a value of this variable usually requires one system call. To avoid a system call, the listen directives must specify addresses and use the bind parameter.

$server_name
name of the server which accepted a request
$server_port
port of the server which accepted a request
$server_protocol
request protocol, usually “HTTP/1.0”, “HTTP/1.1”, or “HTTP/2.0”
$status
response status (1.3.2, 1.2.2)
$tcpinfo_rtt, $tcpinfo_rttvar, $tcpinfo_snd_cwnd, $tcpinfo_rcv_space
information about the client TCP connection; available on systems that support the TCP_INFO socket option
$time_iso8601
local time in the ISO 8601 standard format (1.3.12, 1.2.7)
$time_local
local time in the Common Log Format (1.3.12, 1.2.7)
$uri
current URI in request
```
