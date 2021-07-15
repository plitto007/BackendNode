# Docker

## Copy files in docker container
* To copy a file from the local file system to a container:
```bash
docker cp <src-path> <container>:<dest-path> 
```
* To copy a file from the container to the local file system:
```bash
docker cp <container>:<src-path> <local-dest-path> 
```

Example:
```
docker cp scrapy-cluster_crawler_1:"/usr/src/app/screenshot_ipg_thread_1.png" "/home/manhpv/Desktop/temp/data_1.png"
```