# Back of Envelope Calculations



### Power of 10's and 2's
* $10^3$ ~ Thousand ~ KB ~ $2^(10) $  
* $10^6$ ~ Million ~ MB ~ $2^(20) $  
* $10^9$ ~ Billion ~ GB ~ $2^(30) $  
* $10^12$ ~ Trillion ~ TB ~ $2^(40) $  
* $10^15$ ~ Quadrilion ~ PB ~ $2^(50) $  

### Data units and Object sizes
* 1B = 8bits  
* char: 1B  
* char(unicode), short: 2B  
* int: 4B  
* long: 8B  
* UUID/GUID: 16B  

* Text file with 1K lines ~ (1K lines * 20 words per line * 5 char per word * 1B) ~ 100KB 
* Web page (excluding images) ~ 100KB  
* Full HD Image size = (pixel dimensions) * (pixel depth) / (number of bits per byte) = 1920 * 1080 * 24 * 1/8 ~ 6MB  
* 1 minute Full HD Video size = (frame size) * (frame rate per second) * (compression ratio) * (duration in sec) = $6*10^6$ * 30 * (1/100) * 60 ~  108 MB ~ 100 MB  


### Daily/Monthly volume to QPS (queries per second)
* 1M requests per day => 12 qps
* 1B request per month => 400 qps
* Number of seconds in a day = 86400 ~ 84K = 1M/12

### Latency numbers

|Operation name	                               | Time  |
| -------------------------------------------- | ----- |
|L1 cache reference	                           | 0.5 ns|
|Branch mispredict	                           | 5 ns  |
|L2 cache reference	                           | 7 ns  |
|Mutex lock/unlock	                           | 100 ns|
|Main memory reference	                       | 100 ns|
|Compress 1K bytes with Zippy                  | 10 µs |
|Send 2K bytes over 1 Gbps network             | 20 µs |
|Read 1 MB sequentially from memory            | 250 µs|
|Round trip within the same datacenter         | 500 µs|
|Disk seek                                     | 10 ms |
|Read 1 MB sequentially from the network       | 10 ms |
|Read 1 MB sequentially from disk              | 30 ms |
|Send packet CA (California) ->Netherlands->CA | 150 ms|

### Availability numbers

|Availability %|Downtime per year @100% impact|Downtime per year @10% impact|
| ------------ | ---------------------------- | --------------------------- |
| 99%          | 3.65 days                    | 36.5 days                   | 
| 99.9%        | 8.76 hours                   | 3.65 days                   | 
| 99.99%       | 52.56 minutes                | 8.76 hours                  | 
| 99.999%      | 31.56 seconds                | 52.56 minutes               |

### Cost of Operations
* Read sequentially from HDD: 30 MB/s
* Read sequentially from SSD: 1 GB/s
* Read sequentially from memory: 4 GB/s
* Read sequentially from 1Gbps Ethernet: 100MB/s

### References
https://matthewdbill.medium.com/back-of-envelope-calculations-cheat-sheet-d6758d276b05  
https://youtu.be/-frNQkRz_IU  
http://preservationtutorial.library.cornell.edu/intro/intro-06.html#:~:text=If%20the%20pixel%20dimensions%20are,)%2F8%2C%20or%2018%2C874%2C368%20bytes.  
https://bytebytego.com/courses/system-design-interview/back-of-the-envelope-estimation
