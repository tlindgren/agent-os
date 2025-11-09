## Security Best Practices

### AI as Code Reviewer Approach

Treat yourself as a code reviewer, not just a code generator. Proactively identify security issues and flag areas that need human verification before deployment.

### 🚨 Critical Security Rules

#### Secret and API Key Management

**Never write real secrets in code:**
- ❌ **NEVER:** Write actual API keys, tokens, passwords, or secrets directly in code files
- ❌ **NEVER:** Include real secrets in example code or comments
- ❌ **NEVER:** Place secrets or sensitive configuration in frontend/client-side code

**Always use secure patterns:**
- ✅ **DO:** Use placeholder patterns like `YOUR_API_KEY_HERE`, `process.env.API_KEY`
- ✅ **DO:** Reference environment variables or secret management systems
- ✅ **DO:** Include clear comments indicating where humans should add actual secrets

```typescript
// ✅ CORRECT: Reference environment variable
const apiKey = process.env.GEMINI_API_KEY;

// ✅ CORRECT: Clear placeholder with instructions
const apiKey = "YOUR_API_KEY_HERE"; // Replace with actual API key from environment

// ❌ INCORRECT: Never include real keys
const apiKey = "AIzaSyC-****-****************"; // NEVER DO THIS
```

#### Authentication and Authorization

**Never generate mock authentication that looks production-ready:**
- ❌ **NEVER:** Implement authentication with hardcoded user lists or simple checks
- ❌ **NEVER:** Skip authorization checks with comments like "// TODO: Add auth later"
- ✅ **DO:** Implement real authentication systems using established libraries
- ✅ **DO:** Include proper authorization checks for protected resources
- ✅ **DO:** Mark any temporary implementations clearly for replacement

```typescript
// ❌ INCORRECT: Mock auth that looks production-ready
function authenticate(username: string, password: string): boolean {
  return username === "admin" && password === "password";
}

// ✅ CORRECT: Real authentication implementation
async function authenticate(email: string, password: string): Promise<User | null> {
  const user = await db.user.findUnique({ where: { email } });
  if (!user) return null;
  
  const isValid = await bcrypt.compare(password, user.hashedPassword);
  return isValid ? user : null;
}

// ✅ CORRECT: Clearly marked temporary mock
// ⚠️ TEMPORARY MOCK - REPLACE WITH REAL AUTH BEFORE PRODUCTION
function mockAuthenticate(username: string): User {
  console.warn("🚨 Using mock authentication - NOT FOR PRODUCTION");
  return { id: "test-user", username, role: "user" };
}
```

### Default Security Posture

**Always implement these practices:**
- Treat all user input as untrusted
- Use parameterized queries/prepared statements for database operations
- Implement proper authentication checks before data access
- Validate and sanitize on both client and server
- Avoid exposing sensitive data in error messages or client code
- Consider OWASP Top 10 vulnerabilities in all implementations

### Proactive Security Review

**Questions to raise during code generation:**
- "This endpoint handles user data - should we implement rate limiting?"
- "I notice this component accepts unsanitized input. Should we add validation?"
- "This approach stores sensitive data client-side. Is that intentional?"
- "This dependency was suggested by AI - recommend verifying it's actively maintained."

**Before generating large amounts of code, ask:**
- "What's the expected scale/traffic? That affects security implementation."
- "Is this user-facing or admin-only? Security requirements differ significantly."
- "Should this be optimized for performance or security right now?"
- "Do you want me to generate tests for security scenarios?"

### Input Validation and Error Handling

**Validate everything:**
- Always validate and sanitize user inputs
- Use parameterized queries for database operations
- Implement proper CORS policies
- Validate file uploads and restrict file types
- Never trust user input without validation

**Safe error handling:**
- Never expose sensitive information in error messages
- Never return stack traces or internal details to clients
- Log detailed errors server-side for debugging
- Return generic error messages to clients

```typescript
// ❌ INCORRECT: Exposes sensitive information
catch (error) {
  return res.status(500).json({
    error: error.message, // Could expose database details, file paths, etc.
    stack: error.stack
  });
}

// ✅ CORRECT: Safe error handling
catch (error) {
  console.error("Database operation failed:", error); // Log internally
  return res.status(500).json({
    error: "An internal error occurred. Please try again later."
  });
}
```

### Data Protection

**Protect sensitive data:**
- Implement proper data encryption for sensitive information
- Use HTTPS for all data transmission
- Implement proper session management
- Follow data minimization principles
- Never store passwords in plain text
- Never log sensitive user data

### Dependencies and Package Security

**Use trusted packages:**
- Recommend official, well-maintained packages
- Suggest regular dependency updates
- Recommend security scanning tools
- Never suggest packages with known vulnerabilities
- Avoid packages from untrusted sources

### When Mocking is Acceptable

**Limited scenarios only:**
- Unit tests and test environments
- Development prototypes (clearly marked)
- Educational examples (with explicit warnings)

**Always mark mock code clearly:**
```typescript
// 🚨 DEVELOPMENT ONLY - REMOVE BEFORE PRODUCTION
// ⚠️ MOCK IMPLEMENTATION - REPLACE WITH REAL AUTH
// TODO: CRITICAL - Implement real authentication before deployment
// FIXME: Security vulnerability - using mock data
```

### Production Readiness Checklist

**Before any authentication/authorization code is considered complete:**
- [ ] Real user authentication system implemented
- [ ] Password hashing/encryption in place
- [ ] Session management configured
- [ ] Authorization checks on all protected routes
- [ ] Input validation on all endpoints
- [ ] Error handling doesn't expose sensitive info
- [ ] Security headers configured
- [ ] HTTPS enforced in production

### Human Responsibilities

**The human developer must:**
- Provide actual API keys and secrets through secure means
- Review all generated authentication/authorization code
- Ensure production deployments meet security requirements
- Configure proper environment variables and secrets management
- Conduct security reviews before production deployment

### AI Assistant Security Responsibilities

**AI assistants must:**
- Never generate or guess real secrets
- Always implement real authentication when requested
- Clearly mark any temporary/mock implementations
- Follow secure coding practices
- Prompt for human review of security-critical code
- Refuse to implement obviously insecure patterns
- Flag potential vulnerabilities proactively
