dd



###
```
server {
        listen 443 ssl;
        ##
        # This is the only set of keys that matter,
        # if you want to set a valid cert, here is
        # where you would do it.
        ##
        ssl_certificate /tmp/server.crt;
        ssl_certificate_key /tmp/server.key;

        ##
        # This makes sets what host header to answer
        # to, default means everything, can be useful
        # if you want to have more than one host
        # for each of your attack tools
        ##
        server_name default;

        ##
        # Next 3 are the callback URLs for Empire
        ##

        location /admin/get.php {
                proxy_pass https://192.168.1.100:2443;
        }

        location /news.asp {
                proxy_pass https://192.168.1.100:2443;
        }

        location /login/process.jsp {
                proxy_pass https://192.168.1.100:2443;
        }

        ##
        # Stager URIs for Empire
        ##    
        location ~ ^/index\.(asp|php|jsp)$ {
                proxy_pass https://192.168.1.100:2443;
        }

        ##
        # Regular checkin regex (20 or more long URI with upper, lower and -_) ending in /
        ##
        location ~ "^/([a-zA-Z0-9\-\_]{20,})/$" {
                proxy_pass https://192.168.1.100:1443;
        }

        ##
        # Stage URI (5 characters, upper, lower, and -_) with no extension
        ##
        location ~ "^/([a-zA-Z0-9\-\_]{5})$" {
                proxy_pass https://192.168.1.100:1443;
        }

        ##
        #  Web Delivery URI, not required if not staged through web_delivery module
        ##
        location /logoffbutton {
                proxy_pass https://192.168.1.100:1443;
        }

        ##
        # Catch all to toss the rest at google
        ##
        location / {
                proxy_pass https://www.google.com;
        }
}

```
