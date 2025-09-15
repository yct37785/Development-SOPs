# Commenting Guidelines
Standardized commenting workflow for all projects, applies to all languages (TypeScript, C++, Java, etc.) with only syntactic adjustments per ecosystem.

Each function, method, class, or exported/public symbol must have a **block comment** immediately above its declaration.

A block comment follows this generalized structure:
```javascript
/******************************************************************************************************************
 * <[ASYNC] Brief imperative summary>
 *
 * @template <T> - <generic type parameter explanation>
 *
 * @property <field> - <property description>
 *   - <nestedField>: <obj type> - <nested description>:
 *     + <deepField>: <type> - <deep nested description>
 *
 * @param <name> - <parameter description>
 *   - <nestedList>: <list type> - <list description>:
 *     + <listElem>: <type> - <list elem description>
 *
 * @return - <return value description>
 *
 * @throws {<ErrorType>} <condition when error occurs>
 *
 * @usage
 * ```<language>
 * <minimal working example>
 * ```
 ******************************************************************************************************************/
```

## General Conventions
### Brief
- Always begin with a short, imperative summary.
- Prepend ```[ASYNC]``` if the function or method is asynchronous.
- For classes/services/types, describe the entity’s purpose, guarantees, and typical use cases.

### Templates ```@template```
- Use ```@template``` to document generic type parameters (e.g., ```T```, ```K```, ```V```).
- One line per type parameter is preferred.
- Describe:
  - What the type represents (domain meaning).
  - Any constraints (```extends …```) and defaults (```= …```).

### Fields (```@property```, ```@param```, ```@return```)
- Top level field:
  - No need to delcare type as it is visible/can be infered from the signature.
  - Syntax: ```@<field> <name> - <description/constraints>```
- Nested field:
  - Always specify the type since they are not visible in the signature.
  - Syntax: ```@param <name>: <type> - <description/constraints>```
  - Use ```-``` for first-level nesting, ```+``` for deeper nesting for readability.
- Optional fields should be marked with ```?```.

### Properties ```@property```
- For documenting types, interfaces, classes, providers, or services.
- Follow field conventions specified above.

### Parameters ```@param```
- For documenting function or constructor parameters.
- Follow field conventions specified above.

### Return ```@return```
- Document all function or constructor parameters.
- Top level syntax: ```@return - <description of return>```
- Otherwise follow field conventions specified above.

### Throws ```@throws```
- Document if/when and why the error occurs.
- Syntax: ```@throws {<ErrorType>} <if/when/why>```

### Usage ```@usage```
- Optional section for minimal working examples.
- Use fenced code blocks with the appropriate language identifier (```ts```, ```cpp```, ```java```, etc.).
- Show only the minimal snippet needed to demonstrate correct usage.

### Placement
- Comment blocks must appear immediately above the symbol.
- Each exported/public symbol must be documented.
- Internal helpers should be documented if they handle validation, I/O, or domain-specific logic.

## Comment Block For Functions
Functions and methods should be documented with block comments directly above their declaration.
Follow the **General Conventions** for ```@template```, ```@param```, ```@return```, ```@throws```, and ```@usage```.

Primitive/typed params/return type. Synchronous function:
```javascript
/******************************************************************************************************************
 * <Brief imperative summary>
 *
 * @param <name> - <description/constraints>
 * @param <param_1> - <description of param>
 * @param <param_2>? - <description of param>
 *
 * @return - <description of return>
 *
 * @throws {<ErrorType>} <when/why>
 *
 * @usage
 * ```<language>
 * <minimal working code snippet>
 * ```
 ******************************************************************************************************************/
```

With generic/object params/return with nested fields type. Asynchronous function:
```javascript
/******************************************************************************************************************
 * [ASYNC] <brief imperative summary>
 *
 * @template T - <what T represents / constraint / default if applicable>
 *
 * @param <objectParam> - <description of object>:
 *   - <field_1>?: <type> - <description>
 *   - <nestedList>: <list type> - <list description>:
 *     + <listElem>: <type> - <list elem description>
 *
 * @return - <description of return>:
 *   - <field_1>?: <type> - <description>
 *   - <nestedField>: <obj type> - <nested description>:
 *     + <deepField>: <type> - <deep nested description>
 *
 * @throws {<ErrorType>} <when/why>
 *
 * @usage
 * ```<language>
 * <minimal working code snippet>
 * ```
 ******************************************************************************************************************/
```

## Comment Block For Classes / Services / Providers
Classes, services, providers, dependency-injection containers, context managers, or equivalents must have a block comment directly above their declaration.
Follow the **General Conventions** for ```@template```, ```@param```, ```@property```, ```@throws```, and ```@usage```.
```javascript
/******************************************************************************************************************
 * <brief imperative summary of the class/service/provider>
 *
 * @template T - <what T represents / constraint / default if applicable>
 *
 * @param <ctorOrProps> - <constructor args or provider props>
 *
 * @property <field_1> - <meaning/constraints>
 * @property <field_2>? - <optional/defaults>
 *
 * @throws {<ErrorType>} <when/why>
 *
 * @usage
 * ```<language>
 * <minimal working example showing instantiation/wrapping/consumption>
 * ```
 ******************************************************************************************************************/
```

### Notes
- Only document constructor parameters in the class block.
- Individual methods inside the class must be documented separately using the **function block format**.

## Comment Block For Types / Interfaces
Types, interfaces, structs, or equivalent schema definitions must have a block comment directly above their declaration.
Follow the **General Conventions** for ```@template```, ```@property```, and ```@usage```.
```javascript
/******************************************************************************************************************
 * <brief imperative summary of the type/interface and its contracts>
 *
 * @template T - <what T represents / constraint / default if applicable>
 *
 * @property <field_1> - <meaning/constraints>
 * @property <nestedObj> - <nested description>:
 *   - <nestedField_1>: <type> - <description>
 *   - <nestedList>?: <list type> - <list description>:
 *     + <listElem>: <type> - <list elem description>
 *
 * @usage
 * ```<language>
 * <minimal example constructing/consuming the type/interface>
 * ```
 ******************************************************************************************************************/
```
