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

|Operation name	                              | Time  | Compare |
| --------------------------------------------- | ----- | ------- |
|L1 cache reference	                           | 0.5 ns|         |
|Branch mispredict	                           | 5 ns  |         |
|L2 cache reference	                           | 7 ns  | 14X L1  |
|Mutex lock/unlock	                           | 100 ns|         |
|Main memory reference	                        | 100 ns|         |
|Compress 1K bytes with Zippy                   | 10 µs |         |
|Send 1K bytes over 1 Gbps network              | 10 µs |         |
|Read 4 KB randomly from SSD                    | 150 µs|         |
|Read 1 MB sequentially from memory             | 250 µs|         |
|Round trip within the same datacenter          | 500 µs|         |
|Read 1 MB sequentially from SSD                | 1 ms  | 4X mem  |
|Disk seek                                      | 10 ms |         |
|Read 1 MB sequentially from network (1GBPS)    | 10 ms |         |
|Read 1 MB sequentially from disk               | 30 ms | 120X mem|
|Send packet CA (California)->Netherlands->CA   | 150 ms|         |

### Availability numbers

|Availability %|Downtime per year @100% impact|Downtime per year @10% impact|
| ------------ | ---------------------------- | --------------------------- |
| 99%          | 3.65 days                    | 36.5 days                   | 
| 99.9%        | 8.76 hours                   | 3.65 days                   | 
| 99.99%       | 52.56 minutes                | 8.76 hours                  | 
| 99.999%      | 31.56 seconds                | 52.56 minutes               |

### Cost of Operations per second
* Read sequentially from HDD: 30 MB/s
* Read sequentially from 1Gbps Ethernet: 100MB/s
* Read sequentially from SSD: 1 GB/s
* Read sequentially from memory: 4 GB/s
* 6-7 world-wide round trips per second
* 2,000 round trips per second within a data center


### Bill of Materials calculations
#### SLO
* Full HD image 200ms 99.9 pctl
#### Available infrastructure
We are provided below infrastructure to develop a photo management service
* Network SLA is 99.99%
* Storage SLA for read is 100 ms at 95 pctl and for write is 200 ms at 95 pctl.
* Any number of HDD servers with 8 cores, 24GB RAM, 2X2TB HD, 1GB ethernet.
* Any number of SSD servers with 8 cores, 24GB RAM, 2TB SSD, 1GB ethernet.
* NIC capacity - 1000 MB/s
#### Estimations
* 1M daily users with 50 uploads, 5 searches and one download per search. 
* Write volume = 1M*50 per day ~ 600 qps
* Daily storage = 1M*50*5MB ~ 250TB
* Monthly storage = 7.5PB
* Thumbnail search volume = 1M*5 per day ~ 60 qps
* Download volume = 1M*5 per day ~ 60 qps
#### Upload service
* Bandwidth
    * Incoming picture data = 600 qps * 5MB = 3000MBps => 3 NICs
    * Theoritical max per NIC = 1000MBps / 5MB = 200 qps
* Timing
    * Storage write ~ 200ms and is non blocking.
    * Blocking network time = 2 (in and out) * 5MB / 1000MBps = 10ms 
    * Assuming index search call ~ 1ms
    * Max rate of picture upload on each NIC = 1000MBps/11ms or 91 qps => 600 qps /91 qps or 7 NICS would be required.

#### Thumbnail service
* Bandwidth
    * 60 qps with 10 results per search of 256KB each => 150 mbps
* Timing
    * Storage read ~ 100 ms and is non blocking.
    * Blocking network time = 10 * 256KB / 1000MBps ~ 2.5ms 
    * Assuming index search query ~ 1ms
    * Max result page load rate on each NIC = max 1000MBps/3.5ms or 286 qps so one NIC should be sufficient to handle 60 qps.

#### Download service
* Bandwidth
    * Download picture data = 60 qps * 5MB = 300mbps => one NIC
* Timing
    * Storage read ~ 100 ms and is non blocking.
    * Blocking network time = 2 (in and out) * 5MB / 1000MBps = 10ms
    * Wait time per query = 110 ms which is less than our SLO.
    * Max rate of picture download per NIC = 1000MBps/10ms ~ 90 qps so one NIC should be sufficient.

#### Index service
* Bandwidth
    * Metadata per photo ~ 8KB
    * For Upload we would need bandwidth of 600 qps * 8KB = 4.2 mbps
    * For Search we would need bandwidth of 60 qps * 10 results per page * 8KB = 4.2 mbps
    * One NIC is sufficient based on bandwidth requirements.
* Storage
    * Index storage for 30 days = 1M daily users * 50 uploads per user * 30 days * 8KB = 12TB
    * Need 3 HDD or 6 SSD.
    * SSD are more future proof but HDD is cheaper.
    * For HDD seek time ~ 2ms + read 8KB sequentially ~ 0.04 ms ~= 2.04 ms
    * For SSD random 4K read ~ 0.02ms + 4K r/w sequentially ~ 0.00003ms ~= 0.02 ms
    * Both are within latency range so cheaper makes sense.
 

#### Load balancer
* Upload ~ 600 qps * 5MB ~ 3000 mbps
* Thumbnail ~ 60 qps * 10 * 8KB ~ 4.2 mbps
* Download ~ 60 qps * 5MB ~ 300 mbps
* Total ~ 3304.2 mbps
* Require 4 NICS.

Final requirements for one datacenter assuming buffer of 25%
|             |Bandwidth|Timing|Storage|Final|
|-------------|---------|------|-------|-----|
|Load balancer|5        |N/A   |N/A    |5    |
|Upload       |4        |6     |N/A    |6    |
|Thumbnail    |1        |1     |N/A    |1    |
|Download     |2        |2     |N/A    |2    |
|Index        |2        |N/A   |4HDD   |4    |
|Total        |N/A      |N/A   |N/A    |18   |

### References
https://matthewdbill.medium.com/back-of-envelope-calculations-cheat-sheet-d6758d276b05  
https://youtu.be/-frNQkRz_IU  
http://preservationtutorial.library.cornell.edu/intro/intro-06.html#:~:text=If%20the%20pixel%20dimensions%20are,)%2F8%2C%20or%2018%2C874%2C368%20bytes.  
https://bytebytego.com/courses/system-design-interview/back-of-the-envelope-estimation
https://youtu.be/ohtqI3AHR0k
