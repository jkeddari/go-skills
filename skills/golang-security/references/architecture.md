# Security Architecture

Security boundaries should make identity, authority, resource ownership, and failure behavior explicit. Network location alone is not an authorization decision.

## Trust-boundary workflow

For each entry point:

1. identify who can supply the request and through which transport;
2. authenticate the principal using the project's selected, reviewed mechanism;
3. authorize the specific action on the specific resource;
4. validate size, shape, and semantic constraints before privileged effects;
5. propagate only the minimum identity data required by downstream code;
6. record the decision without logging credentials or sensitive payloads.

Authentication answers who the principal is. Authorization answers whether that principal may perform this action now. Keep them separate so a valid identity cannot bypass resource-level checks.

## HTTP boundary

Standard-library middleware can enforce transport-independent controls:

```go
func LimitBody(max int64, next http.Handler) http.Handler {
	return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
		r.Body = http.MaxBytesReader(w, r.Body, max)
		next.ServeHTTP(w, r)
	})
}

func SecurityHeaders(next http.Handler) http.Handler {
	return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
		w.Header().Set("X-Content-Type-Options", "nosniff")
		w.Header().Set("Referrer-Policy", "strict-origin-when-cross-origin")
		next.ServeHTTP(w, r)
	})
}
```

Set content security, framing, cross-origin, and transport policies according to the actual application. Copying a restrictive header set without testing can break legitimate clients; omitting it can leave browser behavior unconstrained.

## TLS

Use `crypto/tls` and `crypto/x509` for transport configuration. Set a supported minimum version, verify peer identity, and rotate certificates without requiring a full application redesign. Never set `InsecureSkipVerify` in production to work around trust-store failures.

For mutual TLS, distinguish successful certificate validation from application authorization. A valid workload certificate still needs an explicit mapping to allowed operations.

## Credentials and tokens

- Keep signing, encryption, and session keys out of source and logs.
- Validate issuer, audience, expiry, intended algorithm, and replay constraints appropriate to the token format.
- Use short-lived credentials and a documented rotation and revocation path.
- Do not implement a new token or password format casually; use a project-approved, reviewed implementation behind a narrow local interface.
- Compare secret values in constant time when equality itself is security-sensitive.

## Resource-level authorization

Make the resource identifier and action visible to policy code:

```go
type Authorizer interface {
	Allowed(ctx context.Context, principal Principal, action string, resource Resource) error
}
```

Test cross-tenant identifiers, deleted or disabled principals, stale roles, missing ownership, batch endpoints, and indirect references. A route-level role check is insufficient when records belong to different users or tenants.

## Availability controls

Bound request bodies, parse depth, decompression ratio, concurrency, queue length, retries, and outbound time. Apply limits per relevant identity or resource where a global limit would let one client starve everyone else. Ensure rejected work is cheap to reject.

## Logging and failure behavior

Return stable public errors and keep detailed internal causes in protected logs. Audit both denied and privileged operations with bounded identifiers, but never include raw credentials, cookies, tokens, or full request bodies.
