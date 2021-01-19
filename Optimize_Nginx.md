# Optimize Nginx

## Adjust worker_processes
* "worker_processes" tells your virtual server how many cores are assigned to each processor so that Nginx can manage the concurrent requests in an optimized way

* The default Nginx configuration path is: /etc/nginx/nginx.conf

* Find number of cores in Cpu 
    ```
    grep processor /proc/cpuinfo | wc –l
    ```
* Should set worker_processes value of number of cores

##  Maximize worker_connections
* Worker connections tells worker processes how many clients can be served simultaneously by Nginx.

* The maximum number for the worker connections setting is 1024 and it’s best to use this to get the full potential from Nginx.

* So set "worker_connections" to 1024 x (number of cores)