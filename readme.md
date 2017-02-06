Slightly modified version of paris-traceroute, in order to be compatible with the [MONROE platform.](https://www.monroe-project.eu/) [The original unmodified code can be found here.](https://code.google.com/p/paris-traceroute/source/checkout)

This binary can work inside the containers of MONROE. In the original version of paris-traceroute there is no option to choose which interface will be used. In this version, flags to set the interface and source IP of the transmitted packets have been added. Setting the interface is obligatory. If it is not set, the program will crash. This is done of purpose, since if the interface is chosen automatically, it will probably not be what the experimenter intended to use. The source IP flag is optional. Please note that just setting the IP flag to the IP of an interface without setting the interface flag will not work either. This is done on purpose as well, since it is possible multiple interfaces to have the same IP within the MONROE network namespace. If the IP flag is not set, the source IP is set to be the IP of the chosen interface.

## How to set the flags
```
-C  --nodeIPArgument     provide the source IP
-O  --nodeInterfaceArgument provide the source interface (obligatory)
```

## Example usage (from within a MONROE container)
```
root@b59e69a56297:/# paris-traceroute -O op2 -C 192.168.1.127 8.8.8.8
traceroute [(192.168.1.127:33456) -> (8.8.8.8:33457)], protocol udp, algo hopbyhop, duration 18 s
 1  192.168.1.1 (192.168.1.1)  2.946 ms   0.553 ms   0.559 ms 
 2  * * *
 3  10.133.17.29 (10.133.17.29)  83.259 ms   136.577 ms   82.050 ms 
 4  10.133.17.14 (10.133.17.14)  78.783 ms   131.510 ms   79.231 ms 
 5  10.133.17.236 (10.133.17.236)  84.243 ms   133.024 ms   79.785 ms 
 6  10.133.17.3 (10.133.17.3)  81.543 ms   139.381 ms   100.263 ms 
 7  83.224.40.186 (83.224.40.186)  89.319 ms   188.926 ms   179.963 ms 
   MPLS Label 24703 TTL=254
 8  83.224.40.185 (83.224.40.185)  82.710 ms   172.438 ms   147.020 ms 
 9  85.205.14.105 (85.205.14.105)  85.179 ms   137.514 ms   125.869 ms 
10  72.14.223.169 (72.14.223.169)  85.609 ms   137.363 ms   118.063 ms 
11  216.239.47.128 (216.239.47.128)  79.567 ms   146.356 ms   145.285 ms 
12  209.85.243.33 (209.85.243.33)  129.615 ms   198.938 ms   269.407 ms 
   MPLS Label 568892 TTL=1
13  64.233.174.143 (64.233.174.143)  108.599 ms   185.810 ms   246.661 ms 
   MPLS Label 692130 TTL=1
14  108.170.234.47 (108.170.234.47)  111.645 ms   825.615 ms   1424.942 ms 
15  * * *
16  google-public-dns-a.google.com (8.8.8.8)  103.087 ms !T2   166.279 ms !T2   224.649 ms !T2 
```
## Notes
In the ideal case,`paris-traceroute` should be run sequentially and preferably when the node is generating little traffic in general. This is because it uses packet capture to detect the replies from the intermediate nodes and background traffic might interfere with this process.
