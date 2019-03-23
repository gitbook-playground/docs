# Common Problems

### CORS issue

If you deploying **Jet Bridge** behind proxy or some web server you can start receiving the following errors in your browser console:

> Access to XMLHttpRequest at '...' from origin 'https://app.jetad.io.io' has been blocked by CORS policy: Response to preflight request doesn't pass access control check: No 'Access-Control-Allow-Origin' header is present on the requested resource.

Normally you shouldn't have this issue as **Jet Bridge** automatically adds the appropriate **CORS** headers to all responses.

#### Behind Nginx

To fix **CORS** issue for **Nginx** add the following to your server config:

{% code-tabs %}
{% code-tabs-item title="my-website.conf" %}
```text
server {
    listen 80;
    ...

    location / {
      ###################################
      # START
      # Add this block to your location
      ###################################
      
      proxy_hide_header 'Access-Control-Allow-Origin';
      proxy_hide_header 'Access-Control-Allow-Methods';
      proxy_hide_header 'Access-Control-Allow-Headers';
      proxy_hide_header 'Access-Control-Expose-Headers';

      if ($request_method = 'OPTIONS') {
        add_header 'Access-Control-Allow-Origin' '*' always;
        add_header 'Access-Control-Allow-Methods' 'GET, POST, PUT, PATCH, DELETE, OPTIONS';
        add_header 'Access-Control-Allow-Headers' 'Authorization,DNT,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Range';
        add_header 'Access-Control-Max-Age' 1728000;
        add_header 'Content-Type' 'text/plain; charset=utf-8';
        add_header 'Content-Length' 0;
        return 204;
      }

      add_header 'Access-Control-Allow-Origin' '*' always;
      add_header 'Access-Control-Allow-Methods' 'GET, POST, PUT, PATCH, DELETE, OPTIONS';
      add_header 'Access-Control-Allow-Headers' 'Authorization,DNT,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Range';
      add_header 'Access-Control-Expose-Headers' 'Content-Length,Content-Range';
      
      ###################################
      # END
      ###################################
      
      ...
      proxy_pass http://webserver;
      ...
    }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}
