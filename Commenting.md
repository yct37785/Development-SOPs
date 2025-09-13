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
 * @return {<Type>} - <description of return>
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
 * @param <name>: <type> - <description/constraints>
 * @param <objectParam>: <obj type> - <description of object>:
 *   - <field_1>: <type> - <description>
 *   - <field_2>?: <type> - <description, optional>
 *   - <nestedObj>: <obj type> - <short description of nested object>:
 *       - <nestedField_1>: <type> - <description>
 *   - <listField>?: <list type> - <description of list items>:
 *       - <elemField_1>: <type> - <description>
 *       - <elemField_2>: <type> - <description>
 *
 * @return {<Type>} - <description of return>:
 *   - <field_1>: <type> - <description>
 *   - <nestedObj>: <obj type> - <short description of nested object>:
 *       - <nestedField_1>: <type> - <description>
 *   - <listField>?: <list type> - <description of list items>:
 *       - <elemField_1>: <type> - <description>
 *       - <elemField_2>: <type> - <description>
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
- Always declare the type (primitive, object, list, etc.).
- Expand objects by listing their nested fields, each with its type.
- Expand arrays/lists/vectors by declaring them as ```Array```, ```List```, ```std::vector```, etc. then expand their element fields below.
- Mark optional fields with ```?```.

#### Return ```@param```
- Always wrap the type in ```{}```.
- Document nested objects and arrays the same way as parameters.

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
 * @param userId: string - unique user identifier (e.g., UUID)
 * @param opts: object - options controlling returned data:
 *   - includeActivity?: boolean - include recent activity if true (default: false)
 *   - limit?: number - max activity items to include (default: 10; range: 1–100)
 * @param req: object - request context:
 *   - headers: object - request headers:
 *       - authToken: string - bearer token used for authorization
 *
 * @return {UserOverview} - resolved overview:
 *   - profile: object - primary identity fields:
 *       - id: string - unique user id
 *       - email: string - user’s email address
 *       - displayName: string - human-friendly display name
 *       - createdAt: string - ISO timestamp of account creation
 *   - activity?: Array - recent activity (if requested):
 *       - id: string - activity id
 *       - type: string - activity type (e.g., LOGIN)
 *       - at: string - ISO timestamp when the activity occurred
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
  // ...implementation...
}
```

#### C++
```cpp
/******************************************************************************************************************
 * [ASYNC] Retrieves a user's overview (profile and optional recent activity).
 *
 * @param userId: std::string - unique user identifier (e.g., UUID)
 * @param opts: Options - options controlling returned data:
 *   - includeActivity?: bool — include recent activity if true (default: false)
 *   - limit?: int — max activity items to include (default: 10; range: 1–100)
 * @param req: Request - request context:
 *   - headers: Headers - request headers:
 *       - authToken: std::string — bearer token used for authorization
 *
 * @return {UserOverview} — resolved overview:
 *   - profile: std::map<std::string, std::string> — primary identity fields:
 *       - id: std::string — unique user id
 *       - email: std::string — user’s email address
 *       - displayName: std::string — human-friendly display name
 *       - createdAt: std::string — ISO timestamp of account creation
 *   - activity?: std::vector<ActivityItem> — recent activity (if requested):
 *       - id: std::string — activity id
 *       - type: std::string — activity type (e.g., LOGIN)
 *       - at: std::string — ISO timestamp when the activity occurred
 *
 * @throws {AuthError} when the auth token is missing, expired, or invalid
 * @throws {NotFoundError} when the user does not exist
 * @throws {RateLimitError} when the request exceeds rate limits
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
 * [ASYNC] Retrieves a user's overview (profile and optional recent activity).
 *
 * @param userId: String - unique user identifier (e.g., UUID)
 * @param opts: Options - options controlling returned data:
 *   - includeActivity?: boolean — include recent activity if true (default: false)
 *   - limit?: int — max activity items to include (default: 10; range: 1–100)
 * @param req: Request - request context:
 *   - headers: Headers - request headers:
 *       - authToken: String — bearer token used for authorization
 *
 * @return {UserOverview} — resolved overview:
 *   - profile: Map<String, String> — primary identity fields:
 *       - id: String — unique user id
 *       - email: String — user’s email address
 *       - displayName: String — human-friendly display name
 *       - createdAt: String — ISO timestamp of account creation
 *   - activity?: List<ActivityItem> — recent activity (if requested):
 *       - id: String — activity id
 *       - type: String — activity type (e.g., LOGIN)
 *       - at: String — ISO timestamp when the activity occurred
 *
 * @throws {AuthException} when the auth token is missing, expired, or invalid
 * @throws {NotFoundException} when the user does not exist
 * @throws {RateLimitException} when the request exceeds rate limits
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
