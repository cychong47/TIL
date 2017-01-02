# Maglev 
* Google( https://research.google.com/pubs/pub44824.html )
* Used from 2008  
* Scalable load balancer
	* Consistent hashing 
	* Connection Tracking 
	* Scale-out model backed by router's ECMP
* Bypass kernel space for performance. 
* Support connection persistence 

# Network Architecture
* DNS - Routers - Maglevs - Service EndPoints. 
* One service is served by one or more VIPs 
	* DNS returns VIP considering geolocation and load of location
* One VIP is served by multiple Maglevs 
	* Router use ECMP to select one Maglev
* One VIP is mapped to multiple Service EndPoints
    * Maglev select Service EndPoint by seletion algorithm and connection tracking table
* Maglev use GRE to send incoming packet to Service EndPoint or another Maglev
	* Send to IP fragment to another special Maglev servers
	* Use only 3-tuple for IP fragment
* Each Service EndPoint use Direct Server Return(DSR)

# Maglev

## Controller
* Responsible for VIP announcement with BGP
* Check health status of forwarder
* If forwarder is not headthy, withdraw all VIP announcements

## Forwarder
* Each VIP has one or multiple backend pools(BP) 
* BP contain physical IP address of the Service EndPoint
* Each BP has specific health checking methods - depends on the service requirement(just reachability or more)  
* Config Manager parse and update configuration of forwarder's behavior based on the **Config Objects**   
* Sharding   
	* Sharding of Maglev enables service isolation - new service or QoS  

### Backend Selection  
* Consistent Hashing distribute loads 
* Record selection in **LOCAL connection tracking table**   
    * Connection tracking table is **not shared** with another Maglev    
	* Does not guarantee consistency on Maglev or Service EndPoint Changes(add/delete)  
* For different traffic type  
    * TCP SYN : select Backend and record it in connection tracking table  
	* TCP non-SYN : lookup connection tracking table  
	* 5-tuple : (maybe) lookup connection tracking table and select backend if not found  
	
# Consistent Hashing
* If Maglev is added or removed, router select different Maglev for the exsiting session - ECMP is changed  
* If one Maglev's local connection tracking table is overflowed, it will lose previous selection    
* To resolve this issues,   
    * Synchronize local connection tracking table between Maglevs -> overhead, overhead, overhead  
	* Consistent hashing for minimize disruption in member changes  
	* Maglev hashing - load balancing and minimal disruption on member changes    

# reference  
* good explanation https://blog.acolyer.org/2016/03/21/maglev-a-fast-and-reliable-software-network-load-balancer/
