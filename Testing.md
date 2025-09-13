# Testing Guidelines
## Unit Tests
### Test File Structure
Every unit test file should follow a consistent top-down order for clarity:

```bash
# Imports
#  - function(s) under test
#  - error types, models, helpers, test utils

# DB reset/init
#  - invoke in-memory DB setup (or mock equivalent)

/******************************************************************************************************************
 * setup
 ******************************************************************************************************************/
# File-level setup
#  - beforeAll hooks (e.g., seed a test user, static fixtures)
#  - shared token generation
#  - constants, test doubles, mocks

/******************************************************************************************************************
 * <functionName>
 ******************************************************************************************************************/
<SUITE BLOCK "<functionName>">

  # Suite-level setup
  #  - beforeEach hooks (reset DB state, clear caches, refresh mocks)

  <TEST CASE BLOCK "returns {<Type>} when <scenario>">
    # Arrange: inputs, context
    # Act: call <functionName>(...)
    # Assert: type/shape of return, essential fields, invariants, side effects

  <TEST CASE BLOCK "throws {<ErrorType>} when <scenario>">
    # Arrange: invalid inputs or forbidden context
    # Act + Assert: calling <functionName>(...) raises {<ErrorType>}

  <TEST CASE BLOCK "returns {<Type>} when <edge scenario>">
    # Optional: additional boundaries, idempotency, special conditions

</SUITE BLOCK>
```

### File Placement & Naming
- One test file per source file, colocated:
  - ```root/Folder_1/MyCode_1.ext``` → ```root/Folder_1/MyCode_1.test.ext```
- One test suite block per exported function in the source file.
- The suite title must be exactly the function name (e.g., ```function_1```).
- For grouped classes or modules, nest suites logically (one per method or key export).

### Coverage Expectations
- Each function’s test suite should cover:
  - **Error paths**
    - Input errors (invalid/missing values, bad formats).
    - Auth/permission errors.
    - Conflict/business rule errors.
  - **Success path**
    - Returns the documented type.
    - All required fields are asserted.
  - **External effects**
    - Database/session/state changes verified.
    - Side-effects (e.g., token rotation, file write, cache flush).
  - **Edge cases**
    - Boundary values (limits, empties, nulls).
    - Idempotency or duplicate calls.
    - Time-related behavior (expiry, ordering).
    
### Guidelines
- **Isolation**
  - No cross-test dependencies; reset shared state between tests.
- **Clarity**
  - One outcome per test case.
  - Test titles read as complete sentences.
- **Consistency**
  - Same ordering of imports/setup/suites across all test files.
  - Prefer AAA (Arrange → Act → Assert) structure in test bodies.
- **Validation**
  - Always validate both the return value/response and the resulting DB state.

## Integration Tests
### Purpose
Integration tests validate that multiple components work together as expected — e.g., controllers + middleware + database + token handling. Unlike unit tests (isolated and atomic), integration tests may simulate realistic user flows and are allowed to span many steps within a single test case.

### Test File Structure
Integration test files follow the same top-down style as unit tests, but emphasize **flows** over **atomicity**:

```bash
# Imports
#  - endpoints/routes or modules under test
#  - database setup/teardown
#  - helpers for tokens, requests, seeding

# DB reset/init
#  - spin up in-memory DB (or dedicated test DB instance)
#  - apply migrations/seed base data

/******************************************************************************************************************
 * setup
 ******************************************************************************************************************/
# File-level setup
#  - beforeAll hooks for global resources (connect DB, start server)
#  - afterAll hooks for teardown (disconnect, cleanup)
#  - shared fixtures: tokens, base users, sample projects

/******************************************************************************************************************
 * <flowName>
 ******************************************************************************************************************/
<SUITE BLOCK "<flowName>">

  # Suite-level setup
  #  - beforeEach hooks (reset collections, reissue tokens)
  #  - arrange consistent test state

  <TEST CASE BLOCK "returns 200 when <happy path scenario>">
    # Scenario scripts are valid here
    # Arrange: register user
    # Act: login → fetch protected resource → update entity
    # Assert: HTTP responses, DB mutations, side effects across steps

  <TEST CASE BLOCK "throws 401 when <error scenario>">
    # Arrange: expired token
    # Act: access protected route
    # Assert: HTTP 401 + no DB changes

</SUITE BLOCK>
```

### File Placement & Naming
- Integration test files grouped under a dedicated folder (e.g., ```tests/integration/```).
- Filenames should describe the **flow** or **feature** area, not a single function:
  - **HappyFlow.test.ext** → end-to-end happy path (auth + resource access)
  - **ProtectedRoutes.test.ext** → route guarding with middleware
  - **Security.test.ext** → negative/security-focused flows
  - **TokenInvalidation.test.ext** → logout, refresh, invalidation logic
- Suite title = flow name (e.g., ```HappyFlow```)
- Test case titles = one-sentence outcomes:
  - "returns 200 when user registers and logs in"
  - "throws 401 when accessing protected route with expired token"

### Coverage Expectations
Integration suites should validate:
- **Happy paths**
  - End-to-end flows with correct setup (e.g., register → login → access → logout).
  - Assertions on HTTP responses, persisted state, returned objects.
- **Security/negative flows**
  - Expired, malformed, or forged tokens.
  - Unauthorized access to protected routes.
  - Replay or reuse attacks.
- **Cross-module interactions**
  - Middleware enforcing auth → controller processing → DB writing/reading.
  - Token issuance in one module respected by another.
- **Side effects**
  - Session invalidation, token blacklisting.
  - DB mutations visible across requests.
    
### Guidelines
- Environment parity
  - Run against an in-memory or dedicated test DB.
  - Mock as little as possible (prefer real middleware + persistence).
- Isolation
  - Reset DB between flows.
  - Fresh tokens/fixtures per flow to avoid leakage.
- Scenario scripts are valid
  - Unlike unit tests, integration test cases can be multi-step scripts.
  - A single test may cover the entire lifecycle (register → login → access → logout).
- Validation
  - Always check both the HTTP response (status codes, headers, JSON body).
  - Validate both the response and the resulting DB state.
- Readability
  - Write test cases as if narrating a user journey.
  - Long flows are acceptable if they clearly match real-world behavior.
