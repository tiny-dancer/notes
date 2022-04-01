# Authentication

> Verifying the identity of a user, process, or device, often as a prerequisite to allowing access to resources in a application.
> - [NIST](https://csrc.nist.gov/glossary/term/authenticate)

Authentication (*AuthN*), often referred to as _logging in_, applies to both humans and machines.  While an identity may be able to authenticate to an application, they may not have access to see or do anything.  This post-authentication access is referred to as Authorization *(AuthZ)* and will be discussed in a following post.

## Engineering Excellence

### Identity Provider (IdP)

The safest and most common way to approach authentication is to offload the activity to another party, called an *Identity Provider (IdP)*.  IdP's will handle authentication activites on your behalf and use protocols to securely transition identities to a application. This can be done through 2 approaches:

- Trusting an external Idp: Google, Facebook, Github or Optum ID
- Being your own Idp: Utilizing Okta, Azure AD, or other services

It is very important to rely on one of the above options unless proven otherwise.  Authentication is not a business differentiator for most applications and custom implementing your own IdP without relying on an external service introduces substantial risk handling secure password storage, password rotation, MFA, auditing, compliance requirements, and more.

### Identity Brokering

The capability to support multiple user-facing IdPs where the application only integrates with one.  Think anywhere you see _multiple_ "Login with.." options _as well_ as the application's own login form.  

This is a very valuable architectural implementation because it enables the IdP to be abstracted from the majority of the application.  Where a user may see 5 IdP options to choose from, the application only worries about 1.  This provides "future-proofing" by enabling the application to add or remove supported logins with minimal to no _application_ impact.

For example, let's say application "awesome" has decided to utilize Okta.  Once completing a one-time integration with Okta, user facing logins can be configured without requiring code changes.  

- "Awesome" app decides to first go-live only supporting google, this configuration can occur in Okta with no code changes.
- 6 months later, users demand the ability to login with GovID, this configuration can occur in Okta with no code changes.  
- Months later, a new customer would like to use their own enterprise login, this configuration can occur in Okta with no code changes.  

> Okta can be substitued for Azure AD B2C, AWS Cognito, Keycloak or other services. See [Tech Landscape](https://techlandscape.optum.com) for enterprise supported services.

## IdP Interactions

### SAML 2.0

Introduced in 2003 utilizing XML, SAML 2.0 is still the most generally supported Single-Sign On protocol for major applications today.  Supporting **Human** only.

### OAuth 2.0

OAuth (2.0) was later released as a lighter weight, JSON, mobile, and machine supported authentication and authorization protocol.  A subset of this protocol, `Client Credentials`, is a primary authentication method for machine to machine communication.  Think anywhere you see `Client ID` and `Client Secret` used to retreive an `Access Token`.  Supports both **Human** and **Machine**, primary used only for **Machine**.

### OpenID Connect

OpenID Connect *(OIDC)* was later released as a super-set of OAuth 2.0 focused on addressing security concerns for *Human Users* of OAuth 2.0.  This is viewed as the most modern authentication protocol and is widely used across the consumer internet space. Think anywhere you see "Login with Google" or "Login with Facebook". It is safe to view OpenID Connect as an eventual successor to SAML 2.0.  Supporting **Human** only.

> NOTE: OAuth 2.0, and inherently OIDC, also supports basic authorization through the use of *scopes*, this will be discussed more in the *authorization* post.

### Custom

A constant option in all applications is to implement your own authentication from the ground up.  This is generally faciliated by framework-level libraries and can be straight forward to start with.  However, as stated earlier this approach is not recommended.  Authentication is not a business differentiator for most applications, be mindful of the added legal and scope risk when deciding this approach.

