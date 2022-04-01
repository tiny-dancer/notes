# Authentication and Authorization

Authentication and authorization are foundational to all systems requiring a personalized experience.  Over the years authentication has followed industry standards that have been widely adopted, from SAML to OpenID Connect,  allowing for a more cohesive and secure internet landscape.  Authorization on the other hand does not have widely adopted protocols and is often implemented as an afterthought when needed, leading to inconsistent user experiences between systems, lack of features and high impact to teams retrofitting these features.

We will discuss what the common terminology and patterns are, how they can be implemented, and why they are important.  Peering into the cutting edge, we will cover the foundational role authentication and authorization play in Zero Trust Networking and how Service Meshes are introducing a standardized implementation.


## Authorization

### RBAC

### ABAC

### UMA 2.0

## Zero Trust Networking

Zero trust Networking is a mature implementation of Attributes Based Access Control.  The system pairs the user with their device to create a *Network Agent* identity.  Intelligent, real-time auth decisions are continuouslly made based on the *Network Agent* and request context (*Location*, *System Component being accessed*, *Time of day*, etc).  

To help appreciate, let's visualize how the system may handle *mg* accessing a system:

- Name: *mg*
- Known device for *mg*: *IOS, mac address: 123*
  - Network Agent ID: *mg/ios/123*
- System known home location for *mg*: *Minnesota, USA*

### Scenario 1

Human user is attempting to log into system as *mg* using known device *ios/123* from their home location *minnesota* with correct password

- **Result**: High trust score, access granted; subsequent actions reflect Network Agent ID: *mg/ios/123*

### Scenario 2

Human user is attempting to log into system as *mg* using unknown device *android/456* from their home location *minnesota* with correct password.

- **Result**: Medium trust score; may be allowed, denied or require step up (manager approval, MFA, etc)

### Scenario 3

Human user is attempting to log into system as *mg* using unknown device *android/789* from *New Zealand* with correct password.

- **Result**: Low/Medium trust score; may be denied or require step up (manager approval, MFA, etc)

### Scenario 4

network agent *mg/ios/123* in their home state *minnesota* performing `WRITE` actions to a component his *permissions* provide access to.

- **Result**: High trust score, action allowed

### Secario 5

network agent *mg/android/456* performing a `READ` action to compenent where *mg* has *permissions* to access.

- **Result**: High trust score, action allowed

### Secario 6

network agent *mg/android/456* performing a `WRITE` action to a compenent where *mg* has *permissions* to access.

- **Result**: medium trust score; action may be allowed, denied or require step up (manager approval, MFA, etc)

> NOTE: These results are provided as examples.  How a trust score is evauluated and what actions are taken are part of system configuration

Implemeting a zero trust system provides immensely deep security and allows for simpler "surrounding" architectures if desired.  For example, It is often found when a system reaches this level of maturity, VPNs are found redundant and decomissioned in favor of publicly available enterprise systems.
