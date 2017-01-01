# Maglev 
* Google
* Used from 2008  
* Scalable load balancer
	* Consistent hashing 
	* Connection Tracking 
	* Scale-out model backed by router's ECMP
* Bypass kernel space for performance. 
* Support connection persistence 

# Network Architecture
* DNS - Routers - Maglevs - Service EndPoints. 
* One service is served by one or multiple VIPs 
	* DNS returns VIP(s)
* One VIP is served by multiple Maglevs 
	* Router use ECMP to select one Maglev
* Maglev select Service EndPoint by seletion algorithm and connection tracking table

# (Local) Connection Tracking Table  
* Once Service EndPoint is selected for a specific session, it is stored in Maglev's local connection tracking table  
* 