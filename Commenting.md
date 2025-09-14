# Commenting Guidelines
Standardized commenting workflow for all projects, applies to all languages (TypeScript, C++, Java, etc.) with only syntactic adjustments per ecosystem.

Each function, method, class, or exported/public symbol must have a **block comment** immediately above its declaration.

## Comment Block
Primitive/typed params/return type. Synchronous function.
```javascript
/******************************************************************************************************************
 * <Brief imperative summary>
 *
 * @param <name> - <description/constraints>
 * @param <param_1> - <description of param>
 * @param <param_2> - <description of param>
 *
 * @return - <description of return>
 *
 * @throws {<ErrorType>} <when/why>
 * @throws {<ErrorType>} <when/why>
 *
 * @usage
 * ```<language>
 * <minimal working code snippet>
 * ```
 ******************************************************************************************************************/
```

With generic/object params/return with nested fields type. Asynchronous function.
```javascript
/******************************************************************************************************************
 * [ASYNC] <brief imperative summary>
 *
 * @param <name> - <description/constraints>
 * @param <objectParam> - <description of object>:
 *   - <field_1>: <type> - <description>
 *   - <field_2>?: <type> - <description, optional>
 *   - <nestedObj>: obj - <short description of nested object>:
 *       + <nestedField_1>: <type> - <description>
 *   - <listField>?: Array - <description of list items>:
 *       + <elemField_1>: <type> - <description>
 *       + <elemField_2>: <type> - <description>
 *
 * @return - <description of return>:
 *   - <field_1>: <type> - <description>
 *   - <nestedObj>: obj - <short description of nested object>:
 *       + <nestedField_1>: <type> - <description>
 *   - <listField>?: Array - <description of list items>:
 *       + <elemField_1>: <type> - <description>
 *       + <elemField_2>: <type> - <description>
 *
 * @throws {<ErrorType>} <when/why>
 * @throws {<ErrorType>} <when/why>
 *
 * @usage
 * ```<language>
 * <minimal working code snippet>
 * ```
 ******************************************************************************************************************/
```

### Conventions
#### Brief
- A short, imperative summary at the top of the block.
- Prepend ```[ASYNC]``` if the function is asynchronous.

#### Parameters ```@param```
- Document all parameters.
- Mark optional fields with ```?```.
- Use ```-``` for normal nesting, ```+``` for deeper fields for readability.
- Expand objects by listing their nested fields.
- For arrays/lists/vectors, declare their primitive types ```Array```, ```List```, ```std::vector```, etc. then, like with objects, expand their element fields below.

#### Parameter Fields Nesting
- **Top-level types:** If type is already declared in the function signature, do not repeat it in the comment.
  - Example: ```req: Request``` → ```@param req - Express request```
- **Nested fields:** Always specify the type, since they are not visible in the signature.
  - Example:
    ```
    @param req - Express request:
      - body: obj - request body:
        + email: string - user's email
         + password: string - user's password
    ```

#### Return ```@return```
- Follow the same conventions as with ```@param```

#### Throws ```@throws```
- Always wrap error types in ```{}```.
- Describe when and why each occurs.

#### Usage ```@usage```
- Optional to include a minimal working snippet.
- Use fenced code blocks with the appropriate language identifier.

#### Placement
- Comment blocks must appear immediately above the symbol.

#### Consistency
- All exported/public functions, methods, and classes must be documented.
- Internal helpers should be documented if they handle validation, I/O, or domain-specific logic.

### Examples
#### TypeScript
```javascript
/******************************************************************************************************************
 * [ASYNC] Retrieves a user's overview (profile and optional recent activity).
 *
 * @param userId - unique user identifier
 * @param opts - options controlling returned data:
 *   - includeActivity?: boolean - include recent activity if true (default: false)
 *   - limit?: number - max activity items to include (default: 10; range: 1–100)
 * @param req - request context:
 *   - headers: obj - request headers:
 *       + authToken: string - bearer token used for authorization
 *
 * @return - resolved overview:
 *   - profile: obj - user profile data:
 *       + id: string - user identifier
 *       + email: string - user’s email
 *       + displayName: string - user’s display name
 *       + createdAt: string - ISO timestamp
 *   - activity?: Array - recent activity (if requested):
 *       + id: string - activity identifier
 *       + type: string - activity type
 *       + at: string - ISO timestamp
 *
 * @throws {AuthError} when the auth token is missing, expired, or invalid
 * @throws {NotFoundError} when the user does not exist
 * @throws {RateLimitError} when the request exceeds rate limits
 *
 * @usage
 * ```ts
 * const overview = await getUserOverview(
 *   "user-123",
 *   { includeActivity: true, limit: 5 },
 *   { headers: { authToken: token } }
 * );
 * console.log(overview.profile.displayName);
 * ```
 ******************************************************************************************************************/
export async function getUserOverview(
  userId: string,
  opts: { includeActivity?: boolean; limit?: number },
  req: { headers: { authToken: string } }
): Promise<{
  profile: { id: string; email: string; displayName: string; createdAt: string };
  activity?: Array<{ id: string; type: string; at: string }>;
}> {
  // implementation...
}
```

#### C++
```cpp
/******************************************************************************************************************
 * [ASYNC] Fetches a user profile and optional recent actions.
 *
 * @param userId - unique user identifier
 * @param includeActivity - flag to include recent actions
 *
 * @return - user overview:
 *   - profile: obj - user profile data:
 *       + id: string - user identifier
 *       + email: string - user’s email
 *       + displayName: string - user’s display name
 *       + createdAt: string - ISO timestamp
 *   - activity?: vector - recent actions (if requested):
 *       + id: string - action identifier
 *       + type: string - action type
 *       + at: string - ISO timestamp
 *
 * @throws {AuthError} when the user is unauthorized
 * @throws {NotFoundError} when the user is not found
 *
 * @usage
 * @code{.cpp}
 * auto fut = getUserOverview("user-123",
 *                            Options{.includeActivity=true, .limit=5},
 *                            Request{.headers=Headers{.authToken=token}});
 * UserOverview overview = fut.get();
 * std::cout << overview.profile["displayName"] << std::endl;
 * @endcode
 ******************************************************************************************************************/
UserOverview getUserOverview(const std::string& userId,
                             const Options& opts,
                             const Request& req);
```

#### Java
```java
/******************************************************************************************************************
 * [ASYNC] Retrieves a user profile and optional recent activity.
 *
 * @param userId - unique user identifier
 * @param includeActivity - flag to include recent activity
 *
 * @return - user overview:
 *   - profile: obj - user profile data:
 *       + id: String - user identifier
 *       + email: String - user’s email
 *       + displayName: String - user’s display name
 *       + createdAt: String - ISO timestamp
 *   - activity?: List - recent activity (if requested):
 *       + id: String - activity identifier
 *       + type: String - activity type
 *       + at: String - ISO timestamp
 *
 * @throws {AuthError} when the user is unauthorized
 * @throws {NotFoundError} when the user does not exist
 *
 * @usage
 * <pre>{@code
 * UserOverview overview = getUserOverview(
 *     "user-123",
 *     new Options(true, 5),
 *     new Request(new Headers(token))
 * ).get();
 * System.out.println(overview.getProfile().get("displayName"));
 * }</pre>
 ******************************************************************************************************************/
public CompletableFuture<UserOverview> getUserOverview(String userId, Options opts, Request req) {
    // ...
}
```
