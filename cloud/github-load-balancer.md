# GitHub Load Balancer(GLB)

## Design summary
* The basic idea in GLB implementation of **Rendezvous hashing is sharing and keeping in sync a forwarding table across all director nodes**. Roughly, this ensures that even when a director node is removed or fails, another director node can take its role and keep dispatching existing connection to the right proxies.

## Overall architecture
* Router - director(L4 LB) - Proxy(L7 LB) - backend server
  * Router use ECMP to select director
  * Director keeps proxy state and forwarding table which maps incoming packet(IP source address to Proxy node)
  * Forwarding table is synchronized between Directors
* Three tier (L4/L7 split strategy)
  * L4(IP source, destination and TCP port)
  * L7(information in application layer usually HTTP)
* UDP tunneling is used between L4 and L7 node

## How to handle L7 node changes
* Variant of Rendezvous hashing : consistent hashing - an algorithm that allows director nodes to agree on which proxy should handle a connection. 
  * This remove service disrupion on L7 node changes(addition/removal)
* Direct Server Return
* For the reference, IPVS use multicast to sync between L4 LB

# reference
* [How GitHub Designed its New Load Balancer](https://www.infoq.com/news/2016/09/github-load-balancer-design)
* [Introducing the GitHub Load Balancer](http://githubengineering.com/introducing-glb/)
* [GLB part 2: HAProxy zero-downtime, zero-delay reloads with multibinder](http://githubengineering.com/glb-part-2-haproxy-zero-downtime-zero-delay-reloads-with-multibinder/)
