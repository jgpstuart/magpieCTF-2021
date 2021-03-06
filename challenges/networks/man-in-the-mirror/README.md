# Man-in-the-Mirror
### Category: Networks
### Author: James Lowther (Articuler)

## Description
And no message could have been any clearer...


## Hints
1. Where is the data getting filtered?

## Solution
This challenge provides a website and a proxy. Going to the website without going through the proxy will return an error stating that you can't get the flag this way. Connecting to the website through the proxy will indicate that the flag was returned, but was filtered out by the proxy before getting sent to you.

1. To solve this challenge, we need to intercept the response from the website that was requested by the proxy before it is sent back to the proxy and filtered out. Somehow we need to get the request before it gets obfuscated by the proxy while still using the auth key generated by the proxy.
2. To do this, write your own intercept proxy in a language like Python3 which forwards all requests to the website and prints out the returned HTML.
3. Next, send a request to your intercept proxy using the given proxy. By forwarding the proxy's request through the intercept proxy you will be able to look at the returned HTML while still using the proxy's authentication credentials. The flag will be in the body of this request.

### Request Normally
```
+----------+   Request   +-----------+   Auth Key   +-------------+
|          | ----------> |           | -----------> |             |
|   HOST   |             |   PROXY   |              |   WEBSITE   |
|          | <---------- |           | <----------- |             |
+----------+  Obfuscated +-----------+     Flag     +-------------+
                 Flag
```

### Request with Intercept Proxy
```
+----------+   Request   +-----------+  Auth Key   +---------------+  Auth Key   +-------------+
|          | ----------> |           | ----------> |               | ----------> |             |
|   HOST   |             |   PROXY   |             |   INTERCEPT   |             |   WEBSITE   |
|          | <---------- |           |             |     PROXY     | <---------- |             |
+----------+  Obfuscated +-----------+             +---------------+    Flag     +-------------+
                 Flag                                     |
                                                          |
                                                          |
                                                          v
                                                         Flag
                                                       Retrieved
```
An example intercept proxy can be found at `solve/intercept-proxy.py`.

## Flag
magpie{1m_st4rt1ng_w1th_th3_m4n_1n_th3_m1ddl3}