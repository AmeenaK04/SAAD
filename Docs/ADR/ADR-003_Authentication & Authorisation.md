## Context and Problem Statement

ABC Limited would like major banking, telecom and airlines to utilise a complaints management system, the system must be able to use authorisation and authentication to enable users of the system
to have access privileges and log-in, as defined in the project’s expectations and user stories.
The CMS must operate in a multi-tenant environment, to ensure that specific users can only access data belonging to their own organisation or authority. 
The system must be capable of providing strong security suitable for banking and telecommunications domains and enable role-based access control/

The key architectural challenge was selecting an authentication & authorisation strategy to ensures robust security and strict role enforcement, while remaining scalable, maintainable, and compatible with a service-oriented architecture.

---

## Considered Options

### Session-Based Authentication

- Real time revocation, therefore deleting or invalidating session record, user access can be revoked. 
- Simple to implement for monolithic web applications
- Poor scalability for distributed systems
- Security risk due to session hijacking via stolen session cookies allowing unauthorised access.

### JWT-Based Authentication with Role-Based Access Control (RBAC)

- Using JSON web token for stateless authentication
- Role & permission information embedded in token claims
- Well suited to RESTful, service oriented architectures
- Single Sign-On and cross-domain authentication. Allowing users to authenticate across multiple domains/services with the same token.

### OAuth / OpenID Connect

- Security: You don't need to store & manage sensitive user passwords.
- Suitable for large-scale identity federation and third-party authentication
- Increased complexity with additional infrastructure and configuration
- If an access token/ refresh token is stolen, unauthorised users could access protected data
- Implements SSO

---

## Decision Outcome

In my CMS system i have chosen **JWT-based authentication combined with Role-Based Access Control (RBAC)**.
Users can authenticate via secure login endpoints and receive a signed JWT containing their identity, role, and tenant context. This token is included with subsequent API requests and validated by the backend to enforce both authentication and authorisation.
Role-based access is enforced at the API layer, ensuring that:
- Consumers can only access their own complaints via through their banking application help desk side.
- Only managers can access aggregated reporting data showing detailed insights of their support people and help desk agents
- System administrators can manage tenant onboarding and user privileges such as adding new admins and viewing the full user list.

---

## Rationale

JWT-based authentication was selected because it supports a stateless REST APIs, which aligns with the CMS’s service-oriented architecture & scalability requirements. By avoiding server-side session storage, the system is capable of scaling horizontally to support the 20 million user base and large user volumes and continuous availability.
Role-Based Access Control enables clear separation of responsibilities between different user roles, reflecting the ABC limited expectations and user stories. Embedding role and tenant context within JWTs simplifies request validation, ensuring consistent enforcement of security.
The approach chosen also supports future extensibility, enabling the CMS to hopefully integrate additional features such as chatbot, without requiring changes to the core authentication mechanism.

---

## Consequences

### Positive Consequences

- Compact, which will reduce overhead when transferring data between the client & server.
- Enforcement of role based permissions aligned with AMBS limited expectations.
- Stateless API design supports horizontal scaling
- Strong alignment with multi-tenant security requirements
- Compatible with various programming languages

### Negative Consequences

- Once a JWT is signed, it cannot be revoked or updated, valid until signature is still not expired
- The secret key used to sign JWTs if it is compromised, unauthorised personal can create forged tokens
