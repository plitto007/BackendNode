# Use scrapy for crawling website data

# Issue

* Error 403 : HTTP status code is not handled or not allowed in scrapy <br/>
    => Adding this code in class which extends "scrapy.Spider"
    ```python
    user-agent = "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.1 (KHTML, like Gecko) Chrome/22.0.1207.1 Safari/537.1"
    ```