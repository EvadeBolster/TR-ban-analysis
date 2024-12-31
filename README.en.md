# Disclaimer
Various bans are not included in this list, the domains are enumerated and fetched from various resources; you can check the methods below.

- USOM bans are not included as they are exhaustive and most of the time incomplete
- Some bans are enforced by the ISPs but are not queryable by BTK and ESB APIs

# Contributing 
The master file that should be contributed to is `yasakli_siteler.csv` please either append your changes at the end of the file or insert them alphabetically. If unsure please create an issue.

# Querying
I used v0 to make a [simple page](https://kulwsmm2saihuybu.vercel.app/) that can query the CSV file

# Methodology & Strategy
DNS resolution is made using a residential ISP's default DNS servers (in this case Superonline's) if the site is blocked, the IP is resolved as `2a01:358:4014:a00::3` and `195.175.254.2` which belong to ESB.

Unfortunately this only works if the domain is banned legitimately, if the domain is illegitimately banned (eg. without case, by USOM etc.) there can be two (so far discovered) outcomes.

## Banned by USOM
USOM bans are already publicly published but this trick is good to know I suppose, when a request is sent to the "ESB AIDIYET" server with the Host header set to an USOM-banned domain the response headers are as follows;
```
HTTP/1.1 307 Temporary Redirect
Via: 1.0 middlebox
Location: http://88.255.216.16/landpage?op=2&ms=http://malicious.domain/
Connection: close
```
You can see that the redirect location is not ESB's but a different party's

## Banned by Misc. Party
I wasn't able to quite figure out who bans these domains and how many domains are shaadowbanned this way, one example I was able to find was OnlyFans.
```
HTTP/1.1 307 Temporary Redirect
Via: 1.0 middlebox
Location: http://aidiyet.esb.org.tr/landpage?ms=http%3A%2F%2Fonlyfans.com%2F
Connection: close
```
You can see that the redirect location is ESB's own server

## Legitimate Bans
If you use this curl method on a domain that is banned legitimately there will be an indeterministic response header that is also present for domains that are not banned.
```
HTTP/1.1 400
Content-Type: text/plain;charset=UTF-8
Content-Length: 0
Date: Tue, 31 Dec 2024 18:13:31 GMT
Connection: close
```
