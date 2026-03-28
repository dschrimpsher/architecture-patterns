## Federated Identity & Multi-Tenant API Flow

This pattern demonstrates:

- Identity broker + external IdP federation
- BFF-based session management
- Short-lived service JWTs for API access
- Downstream token acquisition for external services

### When to Use
- Multi-tenant SaaS platforms
- Federated authentication across partner IdPs
- Systems requiring separation between user session and API authorization

### Key Tradeoffs
- Increased complexity in token management
- Dependency on identity broker availability
- Need for careful session and token lifecycle control
