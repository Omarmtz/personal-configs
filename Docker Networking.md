[Document link ](https://docs.docker.com/config/containers/container-networking/)
- [Video reference](https://www.youtube.com/watch?v=bKFMS5C4CG0&t=700s)
- [Tutorial](https://www.youtube.com/watch?v=5grbXvV_DSk)

## Unbound DNS Recursive 
>[Unbound](https://nlnetlabs.nl/projects/unbound/about/) can be a caching server, but it can also do recursion and keep records it gets from other DNS servers as well as provide _some_ authoritative service, like if you have just a few zones — so it can serve as a stub or "glue" server, or host a small zone of just a few domains — which makes it perfect for a lab or small organization. It's also very popular as a recursive and caching layer server in larger deployments.

Can be recursive dns
1. Write own configuration for using

## DNSMasq 
>[DNSMasq](http://www.thekelleys.org.uk/dnsmasq/doc.html) is a lightweight caching server designed for performance and ease of implementation. It is also packaged with a simple DHCP and TFTP server. It's very popular as part of software packaged for home use and is an underlying piece of some other software you might have used like Clonezilla and Pi-Hole because it can provide all these services as a single small package. Unfortunately, even though it's capable of split-DNS, it is a caching-only server. It can't do recursion (it can't look for another DNS server or handle referrals to or from other servers), and it can't host even a stub domain, so it's not too helpful managing names and addresses.

