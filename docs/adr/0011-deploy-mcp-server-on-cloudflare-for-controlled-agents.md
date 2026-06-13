# ADR 0011: Deploy MCP Server on Cloudflare for Controlled Agents

- Date: 2026-06-13
- Status: Accepted
- Supersedes: ADR 0008 where it required two distinct operators for the initial single-operator portfolio phase.

## Context

`Identity-Service` already implements the machine-admin foundation for MCP-facing operations: service-principal bearer auth, typed admin contracts, idempotent mutation envelopes, audit events, approval state, and risk classification.

The next step is to create the dedicated `mcp-server` repo. It must expose reusable MCP tools for OpenClaw and controlled agents without becoming the source of truth for identity, permissions, approvals, or audit.

ADR 0008 assumed that high-risk approvals would always require a different approver from the requester. The current implemented reality in `Identity-Service` is intentionally narrower: while there is only one operational identity, high-risk actions require a deliberate second confirmation step, not two distinct operators. The two-operator rule remains the target once a second operational identity exists.

## Decision Drivers

- Keep `mcp-server` as a thin operational facade.
- Prefer a free or low-cost deployment target.
- Keep the implementation in TypeScript.
- Avoid duplicating `Identity-Service` authorization, risk, approval, and audit logic.
- Keep v1 safe for controlled agents before opening external clients.
- Preserve architectural context inside the MCP repo through ADRs and agent instructions.

## Decision

The first `mcp-server` implementation will use:

- `TypeScript`;
- Cloudflare Workers as the deployment target;
- Cloudflare Agents SDK MCP support, using the stateless handler path for v1;
- project-local ADR skill installation and `skills-lock.json`;
- an `AGENTS.md` file that fixes the repository boundaries and verification rules.

The v1 MCP endpoint is for controlled agents owned by the portfolio operator. It is protected by a shared bearer token at the MCP boundary and uses a separate `Identity-Service` service principal when calling the admin API.

The initial tool family remains discrete and auth-focused:

- `auth.createUser`;
- `auth.assignProjectRole`;
- `auth.revokeProjectAccess`;
- `auth.revokeSession`;
- `auth.banUser`;
- `auth.unbanUser`;
- `auth.readmitProjectMembership`;
- `auth.listProjectUsers`;
- `auth.getUserAccessStatus`;
- `auth.listPendingApprovals`;
- `auth.decideApproval`.

`mcp-server` must consume `@cesco_valle/identity-contracts/admin` and `@cesco_valle/identity-auth-sdk/admin` instead of hand-writing DTOs or calling the database.

## Approval Semantics

For the current single-operator phase:

- low-risk mutations may complete directly if `Identity-Service` allows them;
- high-risk mutations return `pending_approval`;
- `auth.decideApproval` performs the deliberate second step;
- the same operator may confirm while there is only one operational identity.

When a second operational identity is connected, the two-operator rule should be reintroduced: the requester should not approve their own high-risk operation.

## Consequences

### Positive

- The MCP server stays small, cheap, and aligned with Cloudflare's remote MCP support.
- The authorization and audit source of truth remains `Identity-Service`.
- Controlled-agent v1 avoids premature OAuth complexity.
- ADRs and `AGENTS.md` make future agent work safer and more consistent.

### Negative

- A shared MCP bearer token is simpler than OAuth but less flexible for external clients.
- The single-operator confirmation model has weaker separation of duties than the future two-operator target.
- Cloudflare Workers constraints must be respected by dependencies and tests.

## Implementation Notes

- Do not expose a generic `runAdminAction` tool.
- Do not store secrets in `wrangler.jsonc`, docs, tests, or source code.
- Use `wrangler.jsonc` as the Worker config source of truth.
- Use Cloudflare secrets for `IDENTITY_ADMIN_TOKEN` and `MCP_CLIENT_TOKEN`.
- Keep OAuth, Cloudflare Access, and external-client registration out of v1.

## Related Decisions

- ADR 0003 defines centralized auth with project isolation.
- ADR 0004 separates MCP from application persistence and sources of truth.
- ADR 0008 defines the original auth-admin MCP surface and risk-based approval model.
