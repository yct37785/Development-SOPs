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

### File placement & naming
- One test file per source file, colocated:
  - ```root/Folder_1/MyCode_1.ext``` → ```root/Folder_1/MyCode_1.test.ext```
- One test suite block per exported function in the source file.
- The suite title must be exactly the function name (e.g., ```function_1```).
- For grouped classes or modules, nest suites logically (one per method or key export).

### Coverage Expectations
- Each function’s test suite should cover:
  - **Error paths**
    - Input errors (invalid/missing values, bad formats)
    - Auth/permission errors
    - Conflict/business rule errors
  - **Success path**
    - Returns the documented type
    - All required fields are asserted
  - **External effects**
    - Database/session/state changes verified
    - Side-effects (e.g., token rotation, file write, cache flush)
  - **Edge cases**
    - Boundary values (limits, empties, nulls)
    - Idempotency or duplicate calls
    - Time-related behavior (expiry, ordering)
    
### Guidelines
- Isolation
  - No cross-test dependencies
- Clarity
  - One outcome per test case
  - Test case titles must read as a single sentence as described in the structure above
- Consistency
  - Use the same ordering of imports/setup/suites across all test files
  - Prefer AAA (Arrange → Act → Assert) structure in test bodies
