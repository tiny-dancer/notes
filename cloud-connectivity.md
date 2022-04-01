# Cloud Connectivity

Primary focus should be on enabling a zero trust network.

- This needs enterprise wide buy-in. Authentication and encryption being the first important items to implement

A Service:

- Should be able to register with service discovery
- Send a request to another service
  - Only needs to know the service's registered name (host or ip is retrieved via service registry)
  - Authentication and Encrpytion should be handled by the network "mesh", **not** by the application

## Options

### Public Internet

#### Pro's

- Simplest to implement
- Does not require the purchase of infrastructure
- Does not impact networking configurations across accounts

#### Con's

- Public internet is a best-effort network, which means that connection speed isn’t stable or guaranteed (1).
- Data flowing over the internet is not secure, and can be tracked, intercepted, stolen, or even replaced by malicious data (1).
  - --> MG: turn this con into a myth
- Can be bandwith limitations (2)

### VPN

#### Pro's

- Encrypts the data and makes the connection more secure and resistant to taps and attacks

#### Cons

- Does not solve the performance issues (speed and latency) that are typical of the internet (1).
- Can into problems with resiliency and throughput (2)
- Have Limited Flexibility (2)
- This introduces complexity into the pricing model, which now incorporates per-hour as well as per-GB pricing (1).

### Private Peering / Direct Connect

### Public Peering / Direct Connect

Hybrid connectivity should be exceptional. It can reinforce non-cloud-native thinking and designs. Put simply, it can slow transformation and progress towards cloud-native thinking, designs, and architectures. And specifically within Optum, we have legitimate security concerns with increasing our attack surface to our _core_ network as it stands today.

## Security

UnitedHealth Group's core network is suffering from two primary security issues:

- Overly trusted
- Overly used

**Overly trusted**: Services operating in the core network are under-secured. There is an over-reliance of "being in the core network" as primary security defense. There needs to be a shift to _zero trust_ where every service in core (or another network) acts under the belief that the network has been compromised and fully handles it's own secured perimeter.

**Overly used**: All employee machines are on the same core network. If the core network is compromised, any machine is a target: production services, databases and employee laptops. This means, for example, if an employee laptop is compromised while on the network the attacker then has direct connectivity to any server or database.

These are the primary security issues leading to UnitedHealth Group taking **extreme caution** in adding any private connection into this network.

## Operations

Hybrid connectivity also complicates configurations and planning. Connecting large numbers of virtual networks accounts/subscriptions leads to complexity. The CSPs have limitations for connectivity, routing, address-space and this becomes problematic at scale in large organizations such as UHG. Maintaining reliable DNS resolution across the joined networks adds significant complexity. This connectivity planning creates lead times for delivery and requires ongoing interactions between security teams, on-premises infrastructure service teams, and cloud platform teams along with application stakeholders from business units.

CSPs have additional pricing for hybrid connectivity that is layered atop normal bandwidth charges that is additive to any costs for circuits from connectivity providers.

Hybrid connectivity can have benefits in the form of extending directory services into and across CSPs from on-prem. These are often as one-way trusts. On-premises datacenters can “flex” compute and storage capacity into the public clouds for seasonal or unexpected workload spikes.

### References

1. https://www.stratoscale.com/blog/networking/networking-connectivity-dark-side-hybrid-cloud-architectures/
2. https://www.networkcomputing.com/cloud-infrastructure/cloud-connectivity-methods-and-myths
3. http://www.nuagenetworks.net/blog/best-practices-hybrid-cloud-connectivity/
4. https://blog.highq.com/enterprise-collaboration/whats-difference-public-private-hybrid-cloud
5. https://azure.microsoft.com/en-us/overview/what-are-private-public-hybrid-clouds/?cdn=disable
6. https://www.networkcomputing.com/cloud-infrastructure/network-design-considerations-hybrid-clouds
7. https://datapath.io/resources/blog/beyond-mpls-secure-and-dedicated-cloud-to-cloud-connectivity/
