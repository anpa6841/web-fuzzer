### Running the Program

- make
- ./fuzzer http://example.com search
- python -m http.server

<pre>
MacBook-Air-2:web-fuzzer anish$ python -m http.server
Serving HTTP on :: port 8000 (http://[::]:8000/) ...
::ffff:127.0.0.1 - - [14/Feb/2025 20:31:56] "GET /?search=%27%20OR%20%271%27%3D%271 HTTP/1.1" 200 -
::ffff:127.0.0.1 - - [14/Feb/2025 20:31:56] "GET /?search=..%2F..%2Fetc%2Fpasswd HTTP/1.1" 200 -
::ffff:127.0.0.1 - - [14/Feb/2025 20:31:56] "GET /?search=admin%27%20-- HTTP/1.1" 200 -
::ffff:127.0.0.1 - - [14/Feb/2025 20:31:56] "GET /?search=%3Cscript%3Ealert%28%27XSS%27%29%3C%2Fscript%3E HTTP/1.1" 200 -
::ffff:127.0.0.1 - - [14/Feb/2025 20:31:56] "GET /?search=%2500 HTTP/1.1" 200 -
</pre>


<pre>
MacBook-Air-2:web-fuzzer anish$ ./fuzzer http://127.0.0.1:8000 search
[+] Testing: http://127.0.0.1:8000?search=%27%20OR%20%271%27%3D%271
[+] Testing: http://127.0.0.1:8000?search=..%2F..%2Fetc%2Fpasswd
[+] Testing: [+] Testing: http://127.0.0.1:8000?search=%3Cscript%3Ealert%28%27XSS%27%29%3C%2Fscript%3E
[+] Testing: http://127.0.0.1:8000?search=%2500
http://127.0.0.1:8000?search=admin%27%20--
...
...
...
</pre>


🔨 Usage Examples

1️⃣ Fuzz GET Request (Multiple Params)

./fuzzer http://127.0.0.1:8000 username email

Injects payloads into search, username, and email parameters.

<pre>
MacBook-Air-2:web-fuzzer anish$ python -m http.server
Serving HTTP on :: port 8000 (http://[::]:8000/) ...
::ffff:127.0.0.1 - - [15/Feb/2025 11:57:46] "GET /?username=%3Cscript%3Ealert%28%27XSS%27%29%3C%2Fscript%3E&email=%3Cscript%3Ealert%28%27XSS%27%29%3C%2Fscript%3E HTTP/1.1" 200 -
::ffff:127.0.0.1 - - [15/Feb/2025 11:57:46] "GET /?username=admin%27%20--&email=admin%27%20-- HTTP/1.1" 200 -
::ffff:127.0.0.1 - - [15/Feb/2025 11:57:46] "GET /?username=%2500&email=%2500 HTTP/1.1" 200 -
::ffff:127.0.0.1 - - [15/Feb/2025 11:57:46] "GET /?username=..%2F..%2Fetc%2Fpasswd&email=..%2F..%2Fetc%2Fpasswd HTTP/1.1" 200 -
::ffff:127.0.0.1 - - [15/Feb/2025 11:57:46] "GET /?username=%27%20OR%20%271%27%3D%271&email=%27%20OR%20%271%27%3D%271 HTTP/1.1" 200 -
</pre>

2️⃣ Fuzz POST Request (Multiple Params)

./fuzzer http://127.0.0.1:8000 login user pass --post

Sends payloads in POST body as user=<payload>&pass=<payload>.

<pre>
MacBook-Air-2:web-fuzzer anish$ python -m http.server
Serving HTTP on :: port 8000 (http://[::]:8000/) ...
::ffff:127.0.0.1 - - [15/Feb/2025 11:58:10] code 501, message Unsupported method ('POST')
::ffff:127.0.0.1 - - [15/Feb/2025 11:58:10] "POST / HTTP/1.1" 501 -
::ffff:127.0.0.1 - - [15/Feb/2025 11:58:10] code 501, message Unsupported method ('POST')
::ffff:127.0.0.1 - - [15/Feb/2025 11:58:10] "POST / HTTP/1.1" 501 -
::ffff:127.0.0.1 - - [15/Feb/2025 11:58:10] code 501, message Unsupported method ('POST')
::ffff:127.0.0.1 - - [15/Feb/2025 11:58:10] code 501, message Unsupported method ('POST')
::ffff:127.0.0.1 - - [15/Feb/2025 11:58:10] code 501, message Unsupported method ('POST')
::ffff:127.0.0.1 - - [15/Feb/2025 11:58:10] "POST / HTTP/1.1" 501 -
::ffff:127.0.0.1 - - [15/Feb/2025 11:58:10] "POST / HTTP/1.1" 501 -
::ffff:127.0.0.1 - - [15/Feb/2025 11:58:10] "POST / HTTP/1.1" 501 -
</pre>

### Proxy Demo

- Intercept web traffic on Burp Suite
- python -m http.server
- ./fuzzer http://127.0.0.1:8000 param1 param2 --post --proxy http://127.0.0.1:8080

https://github.com/user-attachments/assets/da5cf73a-799a-4207-b208-c0cc54c6b4aa
