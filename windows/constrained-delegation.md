# Constrained Delegation

Microsoft release an update and safer version of kerberos delegation know as "Contrained Delegation"

The main goal of delegation is to solve the double hop issue, the previous solution of unconstrained Delegation allowed the service to perform authentication to anything in the domain, constrained delegation limit the scope to specific services.

The native kerberos protocol does not support constrained delegation by default, therefore microsoft release 2 extenstions for this feature:

### S4u2self

* Purpose: Allow a service to obtain a kerberos ticket for a user without requiring the user's password&#x20;
* How it works:
  * When a user connect to a service, the service can use s4u2self to request a kerberos ticket for the user directly from the KDC
  * The KDC verify the request and issue a ticket representing user's identy, but now allowing the service to use this ticket ACCESS other service yet
  * This ticket is known as "self-ticket" beacuse it represents the user's identiy but is only usable by the service itself that request it

### S4u2proxy

* Purpose: Allow a service to request access to another service on behalf of a user by using the ticket obtained via S4u2self
* How it works:
  * After getting the "self-ticket", the service will uses s4u2proxy to reqest a **forwardable** ticket from the KDC that allow it to act on behalf of the user when accessing the proxy
  * The KDC will verify with the constain delegation policy to check if this user is allow to delegate to the service

### Summary of extenstions

1\) Step 1 (s4u2self): User authenticate to service A, the service A request a "self-ticket" from the KDC for user, without the need of user interaction of credentials

2\) Step 2(s4u2proxy): The service A use this ticket and request permission from KDC to act on behalf of the user when accessing service B (database). If the constrain delegation policy allow this delegation, the KDC will then issue a service ticket for service A to access service B onbehalf of the user

## # Author

Ik0nw
