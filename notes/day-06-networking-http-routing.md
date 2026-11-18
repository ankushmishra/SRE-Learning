# Day 6 - Networking II: HTTP, routing, and weekly recap

## Session goal

Follow a web request from URL to response, read HTTP request and response
headers, recognize common status codes, and observe a route with `traceroute`.
Use the last 20 minutes to review Days 1-5 and mark topics to revisit.

Suggested time: 90-120 minutes.

## 1. Fundamentals

### 1.1 HTTP request and response

HTTP is an application-layer protocol used by clients and servers to exchange
requests and responses. A URL identifies a scheme, host, optional port, and
path. The client resolves the host (Day 5 DNS), establishes transport
connectivity (often TCP, or QUIC for HTTP/3), and sends an HTTP request. The
server returns a status line, headers, and often a body.

An HTTP request commonly contains a method (`GET`, `POST`, `PUT`, `DELETE`),
path, protocol version, headers, and sometimes a body. Response headers provide
metadata such as content type, cache policy, and server-selected location.

Common status codes:

- `200 OK`: request succeeded.
- `301 Moved Permanently`: resource has a permanent redirect; inspect
  `Location`.
- `404 Not Found`: server did not find the requested resource.
- `500 Internal Server Error`: server encountered an unexpected condition.

Status codes describe the response to one request; a `500` does not identify the
root cause by itself.

### 1.2 Routing and traceroute

Routers forward IP packets toward a destination using routing tables. A route
can pass through several networks. `traceroute` (or `tracepath`) sends probes
with increasing TTL (time to live) values; each router that reduces a TTL to
zero may return a time-exceeded response. A hop may not reply because of
filtering, rate limits, or routing policy. A missing hop does not prove that
traffic stops there.

### 1.3 A URL-to-page flow

For a typical HTTPS page: parse URL -> resolve hostname -> establish a path to
the destination -> negotiate TLS -> send HTTP request -> receive status,
headers, and body -> render the response. Redirects, caches, proxies, and
browser behavior can add steps.

## 2. Practical lab

Use public sites only. `curl -v` output can include cookies or other sensitive
headers, so do not capture authenticated or private URLs and do not share
verbose output without reviewing it.

### 2.1 Capture a verbose request

```bash
mkdir -p ~/sre-lab/day6
cd ~/sre-lab/day6
curl -v https://example.com -o /dev/null 2> curl-example.log
less curl-example.log
```

In curl's verbose output, lines prefixed with `>` are sent by the client; lines
prefixed with `<` are received from the server. Find the request method/path,
`Host`, response status, and response headers such as `Content-Type` and
`Location` if present. Save a copy only after checking it contains no secrets.

Show response headers more directly:

```bash
curl -sS -D - -o /dev/null https://example.com
```

### 2.2 Observe status codes

Use a public HTTP test endpoint to request specific responses:

```bash
curl -sS -o /dev/null -w '%{http_code}\n' https://httpbin.org/status/200
curl -sS -o /dev/null -w '%{http_code}\n' https://httpbin.org/status/301
curl -sS -o /dev/null -w '%{http_code}\n' https://httpbin.org/status/404
curl -sS -o /dev/null -w '%{http_code}\n' https://httpbin.org/status/500
```

Expected codes are the requested values when the test service is reachable.
Network policy or endpoint behavior can vary. `-L` follows redirects; compare
with and without it to see the redirect response versus the final response.

### 2.3 Trace a route

```bash
traceroute -m 20 example.com
```

If the command is unavailable, try `tracepath example.com`. Count the displayed
hop rows. Asterisks mean probes did not receive a response before timeout; they
do not necessarily indicate an outage.

### 2.4 Weekly recap

Spend 20 minutes reviewing Days 1-5. Write one topic you can explain, one
command you can run confidently, and one topic to revisit for each day.

## 3. Review and Day 6 notes

1. What is the difference between a request header and a response header?
2. What does `Location` commonly indicate in a 301 response?
3. Why does a traceroute hop sometimes show `* * *`?
4. What does `curl -v` show that `curl -sS` does not?

- Site tested and saved output file:
- Status codes observed:
- Header I can explain:
- Route and hop count:
- Weekly recap topics to revisit:
- Three things learned:
  1.
  2.
  3.
- Confusing command / resolution:

## Done when

- [ ] I saved and reviewed `curl -v` output for one public site.
- [ ] I can point to the request and response headers.
- [ ] I identified 200, 301, 404, and 500 responses.
- [ ] I ran a route trace and counted the displayed hops.
- [ ] I used the final 20 minutes to review the first five days.

## Further reading

- [curl manual](https://curl.se/docs/manpage.html)
- [HTTP overview (MDN)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview)

