# AAuth

## HTTP Clients Need Their Own Identity

In OAuth 2.0 and OpenID Connect, the client has no independent identity. Client identifiers are issued by each authorization server or OpenID provider — a `client_id` at Google is meaningless at GitHub. The client's identity exists only in the context of each server it has pre-registered with.

API keys are the same model pushed further: a shared secret issued by a service, copied to the client, and used as a bearer credential. The problem is that any secret that must be copied to where the workload runs will eventually be copied somewhere it shouldn't be.

AAuth starts from this premise: every agent has its own cryptographic identity. An agent identifier (`aauth:local@domain`) is bound to a signing key, published at a well-known URL, and verifiable by any party — no pre-registration, no shared secrets, no dependency on a particular server. At its simplest, an agent signs a request and a resource decides what to do based on who the agent is. This identity-based access replaces API keys and is the foundation that authorization, governance, and federation build on incrementally.

## Agents Are Different

Traditional software knows at build time what services it will call and what permissions it needs. Registration, key provisioning, and scope configuration happen before the first request. This works when the set of integrations is fixed and known in advance.

Agents don't work this way. They discover resources at runtime. They execute long-running tasks that span multiple services across trust domains. They need to explain what they're doing and why. They need authorization decisions mid-task, long after the user set them in motion. A protocol designed for pre-registered clients with fixed integrations cannot serve agents that discover their needs as they go.

## What AAuth Provides

- **Agent identity without pre-registration**: A domain, static metadata, and a JWKS establish identity with no portal, no bilateral agreement, no shared secret.
- **Per-instance identity**: Each agent instance gets its own identifier (`aauth:local@domain`) and signing key.
- **Proof-of-possession on every request**: HTTP Message Signatures (RFC 9421) bind every request to the agent's key — a stolen token is useless without the private key.
- **Two-party mode with first-call registration**: An agent calls a resource it has never contacted before; the resource returns `AAuth-Requirement`; a browser interaction handles account creation, payment, and consent. The first API call is the registration.
- **Tool-call governance**: A person server (PS) represents the user and manages what tools the agent can call, providing permission and audit for tool use — no resource involved.
- **Missions**: Optional scoped authorization contexts that span multiple resources. The agent proposes what it intends to do in natural language; the person server provides full context — mission, history, justification — to the appropriate decision-maker; every resource access is evaluated in context.
- **Cross-domain federation**: The PS federates with access servers (AS) — the policy engines that guard resources — to enable access across trust domains without the agent needing to know about each one.
- **Clarification chat**: Users can ask questions during consent; agents can explain or adjust their requests.
- **Progressive adoption**: Each party can adopt independently; modes build on each other.

## What AAuth Does Not Do

- Does not require centralized identity providers — agents publish their own identity
- Does not use shared secrets or bearer tokens — every credential is bound to a signing key and useless without it
- Does not require coordination to adopt — each party adds support independently

## Resource Access Modes

AAuth supports four resource access modes, each adding parties and capabilities:

| Mode | Parties | Description |
|------|---------|-------------|
| Identity-based | Agent, Resource | Resource verifies agent's signed identity and applies its own access control |
| Resource-managed (two-party) | Agent, Resource | Resource manages authorization with interaction, consent, or existing auth infrastructure |
| PS-managed (three-party) | Agent, Resource, PS | Resource issues resource token to PS; PS issues auth token |
| Federated (four-party) | Agent, Resource, PS, AS | Resource has its own access server; PS federates with AS |

## Specifications

- [AAuth Protocol (Internet-Draft)](https://datatracker.ietf.org/doc/draft-hardt-aauth-protocol) — The authorization protocol. Four access modes, three token types, agent governance with missions, clarification chat, and call chaining.
- [HTTP Signature Keys (Internet-Draft)](https://datatracker.ietf.org/doc/draft-hardt-httpbis-signature-key/) — Foundation layer. Well-known key discovery, Signature-Key header for conveying public keying material alongside HTTP Message Signatures (RFC 9421).
- [R3 — Rich Resource Requests (Exploratory)](https://dickhardt.github.io/AAuth/draft-hardt-aauth-r3.html) — Vocabulary-based authorization using formats agents already understand (MCP, OpenAPI, gRPC, GraphQL).

## Implementations

| Language | Repository |
|----------|------------|
| TypeScript | [github.com/hellocoop/AAuth](https://github.com/hellocoop/AAuth) |
| Python | [github.com/christian-posta/aauth-full-demo](https://github.com/christian-posta/aauth-full-demo) |
| Java (Keycloak) | [github.com/christian-posta/keycloak-aauth-extension](https://github.com/christian-posta/keycloak-aauth-extension) |

## Links

- [AAuth Website](https://www.aauth.dev)
- [Playground](https://playground.aauth.dev)
- [Specification Source (GitHub)](https://github.com/dickhardt/AAuth)
- [Website Source (GitHub)](https://github.com/hellocoop/aauth.dev)
