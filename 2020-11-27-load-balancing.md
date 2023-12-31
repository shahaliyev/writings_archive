---
title: How Google Tackles Frontend Load Balancig
description: Google’s Best Practices on Site Reliability Engineering
categories: [computer science]
tags: [sre, google, load balancing, medium]
---

![](https://cdn-images-1.medium.com/max/800/0*ouNajDWQsT5SMHfP)
_Photo by [Taylor Vick](https://unsplash.com/@tvick?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)_


Even if you had an extremely powerful supercomputer that could serve millions of requests per second, you would abstain from having a single point of failure taking care of all the requests. Even if your computer had a consistent supply of power. Google also clearly understands that there are physical constraints of every networking infrastructure (e.g. the speed of light in fiber optic cables), hence, there is the need for using thousands of machines to serve requests.

## What is Traffic Load Balancing

> **“Traffic load balancing** is how we decide which of the many, many machines in our datacenters will serve a particular request.”

An optimal allocation of machines may depend on several factors, such as the hierarchical (global/local) and technical (software/hardware) levels, or the nature of traffic we are dealing with. Google’s [**Site Reliability Engineering book**](https://sre.google/sre-book/load-balancing-frontend/) brings a simplified example to make things clear.

While searching for a query, the most important variable is **latency**, as users are interested in getting their results quickly. When it comes to video uploads, **throughput** is crucial, as users are rather interested in the success of the operation. Therefore, at the global level, the search requests are sent to the nearest datacenter to increase latency, whilst the video upload requests are directed to a link that is underutilized to maximize the throughput.

## What is DNS Load Balancing

Google uses several layers of load balancing, the first of which is DNS load balancing. When a client looks up for an IP address using DNS, simply returning multiple [A](https://support.dnsimple.com/articles/a-record/) or [AAAA](https://support.dnsimple.com/articles/aaaa-record/#:~:text=An%20AAAA%20record%20is%20used,server%2C%20rather%20than%20the%20IPv4.) records and letting the client arbitrarily pick IP addresses would not be optimal. Not only there isn’t enough control over the behavior of the client, but also the client cannot determine its closest address. The mitigation of these problems (if possible) leads to having a more complex server implementation and costly maintenance.

As noted in the book, the **three important implications** of DNS load balancing are the following:

*   Recursive resolution of IP addresses
*   Nondeterministic reply paths
*   Additional caching complications

The [**recursive resolution**](https://www.geeksforgeeks.org/address-resolution-in-dns-domain-name-server/) has a problem that the IP addresses seen by the authoritative nameserver belong to the recursive resolver and not to a user. It only makes reply optimization possible between the resolver and the nameserver. [**EDNS0**](https://en.wikipedia.org/wiki/Extension_mechanisms_for_DNS) extension includes the information about the client's subnet in the DNS query sent by a resolver, allowing the nameserver to return an optimal response from the user’s perspective.

Another implication arises when serving millions of users across different demographic regions. An example is a large Internet Service Provider which might run nameservers for its network from one datacenter, and still have network interconnects in each metropolitan area. As a result, ISP will return a response with the IP address according to their datacenter despite having better options.

Responses are usually cached and forwarded by recursive resolvers within [**time-to-live (TTL)**](https://en.wikipedia.org/wiki/Time_to_live) field limits of the DNS record. That may cause a single authoritative reply to reach a single or thousands of users. Google fixes the problem in two ways: Firstly, the list of DNS resolvers with the approximate user base is constantly updated, which allows tracking the impact on a resolver. Secondly, the geographical distribution of the user base is tracked, which increases the chances of directing users to the best location.

The last implication is related to **caching**. DNS records need a relatively low TTL, as nameservers cannot flush the caches of resolvers. It puts a lower bound on propagating DNS changes to users. Unfortunately, we can’t do much about fixing this problem.

Despite all these problems, DNS is the most effective way to load balancing before a user’s connection starts. However, as there is a 512-byte limit for DNS replies, there exists an upper bound on the number of addresses we can have in a single DNS reply. Virtual IP addresses come in handy here.

## What is Virtual IP Address Load Balancing

[Virtual IP addresses](https://en.wikipedia.org/wiki/Virtual_IP_address) (VIPs) are shared across many devices. A device called [**network load balancer**](https://docs.aws.amazon.com/elasticloadbalancing/latest/network/introduction.html) receives and forwards packets to one of the machines behind VIP, when they further process the request. There are various approaches to deciding which balancer should receive the request.

The most intuitive way is to choose the least loaded backend. Unfortunately, stateful protocols, which use the same backend for the duration of the request, make things problematic here — the balancer must track all connections sent through it to ensure correct allocation.

Another way is to use some parts of a packet to create a connection ID and use it to select a backend. This allows all packets belonging to a single connection to be forwarded to the same backend. However, the problem arises when a backend fails and is removed. The program logic now will map each packet to a different backend and reset almost all of the connections.

The best approach, called [**consistent hashing**](https://www.toptal.com/big-data/consistent-hashing#:~:text=Consistent%20Hashing%20is%20a%20distributed,without%20affecting%20the%20overall%20system.)**,** doesn’t allow both the storing state and resetting all connections when a machine goes down. This mapping algorithm minimizes the disruption when new backends are added or removed from the pool.

Google uses [**packet encapsulation**](https://research.google/pubs/pub44824/) as the current implementation for virtual IP-address load balancing. A load balancer puts a forwarded packet into another IP packet with [**Generic Routing Encapsulation (GRE)**](https://tools.ietf.org/html/rfc1702) when a backend’s address is used as destination.

Packet encapsulation is powerful, however, it inflates packet size, causing overhead. This overhead can require fragmentation of the packet. To avoid fragmentation, a larger [**Maximum Transmission Unit (MTU)**](https://www.imperva.com/blog/mtu-mss-explained/#:~:text=A%20maximum%20transmission%20unit%20%28MTU,each%20packet%20in%20any%20transmission.) size is needed within the datacenter. However, that itself requires large Protocol Data Units.

## Conclusion

For sure, one should do (frontend) load balancing as early and as often as possible. Nevertheless, as we have already discussed, it is easier to say than done, and even minor-looking details can cause great problems for its effective implementation.

## References and Further Reading

*   The chapter this article is based upon, written by Piotr Lewandowski,  
    edited by Sarah Chavis: [https://sre.google/sre-book/load-balancing-frontend/](https://sre.google/sre-book/load-balancing-frontend/)
*   Google’s books on the Site Reliability Engineering: [https://sre.google/books/](https://sre.google/books/)
*   Google Cloud’s External TCP/UDP Network Load Balancing overview: [https://cloud.google.com/load-balancing/docs/network](https://cloud.google.com/load-balancing/docs/network)
*   My previous article on: [Site Reliability Engineering and Google’s Blameless Postmortem Culture](/posts/postmortem)