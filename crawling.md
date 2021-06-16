# Crawl Instagram

## Logging in
* For logging in, At headers first we need to set "user-agent" to mobile:
```
Instagram 123.0.0.21.114 (iPhone; CPU iPhone OS 11_4 like Mac OS X; en_US; en-US; scale=2.00; 750x1334) AppleWebKit/605.1.15
```
* Go to "https://www.instagram.com/" and retrieve "csrftoken" from cookie

* Add that "csrftoken" to header of next request with key "X-CSRFToken"
* Update request headers:
```
Referer: https://www.instagram.com/accounts/login/
x-requested-with: XMLHttpRequest
```
* Finally, make post request to "https://www.instagram.com/accounts/login/ajax/" with this form data body:
```
{'username': your_username, 'password': your_password}
```

## Crawl

* After login we should save the cookie to a text file a reuse for next time crawling

* We don't need to save all the cookie fields. just need 4 fields as in the example below:
```json
{"csrftoken": "xxxxx", "sessionid": "xxxx", "rur": "NAO", "ds_user_id": "xxxx"}
```


# Crawl Facebook
* Do the same as instagram, login and save the cookies to a text file for next time

* Facebook cookies saved as a list of json object like this:

```json
[{
"domain":".facebook.com",
"expiry":000000,
"httpOnly":true,
"name":"xs",
"path":"/",
"sameSite":"None",
"secure":true,
"value":"xxxxxxxxxxxxxxxxxxxxxx"
}, 
{
"domain":".facebook.com",
"expiry":0000000000000000,
"httpOnly":false,
"name":"c_user",
"path":"/",
"sameSite":"None",
"secure":true,
"value":"xxxxxxxxxxxxxxx"
},....]

```