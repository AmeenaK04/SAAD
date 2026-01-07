## Context and Problem Statement


ABC limited requires the CMS system to be used by major companies such as banking, telecom and airlines. The CMS system must be capable of supporting and operating under a multi-tenant architecture, and must ensure strict data isolation between organsations whilst supporting role-based access.
The system must be capable of exposing RESTful API's to support access such as web applications, and for future chat box integration. The backend should be efficient in supporting the whole complaint lifecycle, from the inital consumer logging in a ticket til resolvement.
Based on my NFR the system must be able to seamlessly operate on a load of 20 million user base and yearly 10% increase without any delays whilst maintaining perfomance and reliability. An architectural decision will need to be made to take into account of a backend framework that will support these requriments set by ABC limited.
---

## Considered Options

### Django REST Framework (Django)
- Full-featured web framework with mature REST support
- Built-in authentication, permissions, and request validation, benefital
- Strong support for JWT-based authentication, role-based permissions, and multi-tenant patterns
- Helps build less error-prone, more maintainable and safer API's
  
### Flask
- Able to scale the app for large number of users and workloads: capabale to hold a load of 20 million users
- Lightweight and flexible micro-framework allowing web pages to load quickly and smoothly 
- Would require extensive custom development for authentication, authorization, role separation, and API security
- Higher long-term maintenance overhead

### Node.js (Express)
- Requires multiple third party libraries to achieve equivalent security, authentication, and role management
- Increased architectural complexity for multi-tenant enforcement and fine-grained permissions
- Express provides middleware that automatically parses JSON request bodies, eliminating the need for manual stream processing.
---

## Decision Outcome

Django REST Framework (DRF) was selected as the backend framework for implementing the CMS REST APIs.

The CMS backend has been implemented using DRF to expose service-oriented endpoints such as:
- `/api/staff/login/` with JWT authentication and OTP verification for administrators
- Role-segregated service endpoints for administrators, help desk managers, help desk agents, and consumers and support person.
- Secure, stateless APIs consumed by a React-based frontend

This decision aligns strongly with SOA principles, where functionality is exposed through loosely coupled, well-defined services that can be reused across multiple channels.

---

## Rationale

Django REST Framework was chosen because it:
- Provides robust, built-in security features that reduce implementation risk
- Supports JWT based authentication, which is already implemented in the CMS system
- Enables finegrained role-based access control through distinct API namespaces
- Supports future extensibility for chatbot integration
- Ease of use, due to simply run a pip install command, and add the package to the installed apps.
---

## Consequences

### Positive Consequences
- Accelerated delivery of a secure proof-of-concept
- Reduced security risk through proven authentication and permission mechanisms
- Clean separation between frontend and backend via REST APIs
- Easier scaling and future service expansion
- Strong alignment with SOA and multi-tenant design principles

### Negative Consequences
- Higher initial complexity compared to lightweight frameworks
- Requires careful performance tuning and horizontal scaling strategies at very large scale
- Can slow down your web app due to the added overhead.

