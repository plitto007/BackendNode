# Crawl Instagram
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