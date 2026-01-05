# Security Rules

## XSS (Cross-Site Scripting)

```typescript
// BAD - Dangerous HTML injection
<div dangerouslySetInnerHTML={{ __html: userInput }} />

// GOOD - Sanitize or avoid raw HTML
import DOMPurify from 'dompurify';
<div dangerouslySetInnerHTML={{ __html: DOMPurify.sanitize(userInput) }} />

// BEST - Use text content when possible
<div>{userContent}</div>
```

## Sensitive Data Exposure

```typescript
// BAD - Logging sensitive info
console.log('User data:', { password, ssn, salary });

// BAD - Storing secrets in state/localStorage
localStorage.setItem('apiKey', secretKey);

// GOOD - Never log/store sensitive data client-side
console.log('User action:', { userId, action });
```

## Input Validation

```typescript
// BAD - No validation
const employeeId = params.id;
await sdk.employees.getById(employeeId);

// GOOD - Validate and sanitize inputs
const employeeId = validateUUID(params.id);
if (!employeeId) throw new Error('Invalid employee ID');
await sdk.employees.getById(employeeId);
```

## Authentication & Authorization

- Check that protected routes verify auth status
- Verify role-based access controls are enforced
- Ensure tokens are handled securely
- Never expose auth tokens in URLs or logs

## URL/Redirect Vulnerabilities

```typescript
// BAD - Open redirect
window.location.href = params.redirect;

// GOOD - Validate redirect URLs
const allowedHosts = ['example.com', 'app.example.com'];
const url = new URL(params.redirect);
if (allowedHosts.includes(url.host)) {
  window.location.href = params.redirect;
}
```

## CSRF Protection

- Verify that state-changing requests include CSRF tokens
- Check that cookies have proper SameSite attributes
