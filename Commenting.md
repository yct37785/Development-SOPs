# Commenting Guidelines
Standardized commenting workflow for all projects, applies to all languages (TypeScript, C++, Java, etc.) with only syntactic adjustments per ecosystem.

Each function, method, class, or exported/public symbol must have a **block comment** immediately above its declaration.

A block comment follows this generalized structure:
```javascript
/******************************************************************************************************************
 * <[ASYNC] Brief imperative summary>
 *
 * @template <T> - <Generic type parameter explanation>
 *
 * @property <prop> - <Property description>:
 *   - <nestedProp>  - <Nested description>:
 *     + <deepNestedProp>  - <Deep nested description>
 *     + <deepNestedProp2> - <Deep nested description>
 *
 * @param <param> - <Parameter description>:
 *   - <nestedParam>     - <Nested description>
 *   - <nestedParamList> - <List description>:
 *     + <listElem>        - <List element description>
 *
 * @return - <Return value description>
 *
 * @throws {<ErrorType>} <Condition when error occurs>
 *
 * @usage
 * ```<language>
 * <Minimal working example>
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
  - Syntax: ```@<field> <name> - <Description/constraints>```
- Nested field:
  - Alternate between ```-``` and ```+``` for each layer of nesting for readability.
  - Syntax:
    ```
    - <name> - <Description/constraints>
      + <name> - <Description/constraints>
    ```
- Type declarations:
  - Only specify a type if it is ambiguous or not inferable (i.e., not visible or clearly deducible from the signature).
  - If the type is already declared in the signature (e.g., ```({ nestedField: string, nestedField2: number })```), do not redeclare it.
  - Syntax: ```... <name>: <type> - ...```
- Optional fields:
  - Indicate optionality with ```?```.
- Readability suggestions:
  - Indent descriptions/constraints for fields, with same indentation depth for fields of the same level/nested level if possible.
  - Indentation level is up to individual preferences.
  - Always begin descriptions/constraints with a capital letter.

### Properties ```@property```
- For documenting types, interfaces, classes, providers, or services.
- Follow field conventions specified above.

### Parameters ```@param```
- For documenting function or constructor parameters.
- Follow field conventions specified above.

### Return ```@return```
- Document all function or constructor return values.
- Top-level syntax: ```@return - <Description of return>```.
- Otherwise follow field conventions specified above.

### Throws ```@throws```
- Document if/when and why the error occurs.
- Syntax: ```@throws {<ErrorType>} <If/when/why>```

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
 * @param <name>     - <Description/constraints>
 * @param <param_1>  - <Description of param>
 * @param <param_2>? - <Description of param>
 *
 * @return - <Description of return>
 *
 * @throws {<ErrorType>} <When/why>
 *
 * @usage
 * ```<language>
 * <Minimal working code snippet>
 * ```
 ******************************************************************************************************************/
```

With generic/object params/return with nested fields type. Asynchronous function:
```javascript
/******************************************************************************************************************
 * [ASYNC] <Brief imperative summary>
 *
 * @template T - <What T represents / constraint / default if applicable>
 *
 * @param <objectParam> - <Description of object>:
 *   - <field_1>?  - <Description>
 *   - <nestedList>  - <List description>:
 *     + <listElem>    - <List element description>
 *
 * @return - <Description of return>:
 *   - <field_1>?  - <Description>
 *   - <nestedField>  - <Nested description>:
 *     + <deepField>    - <Deep nested description>
 *
 * @throws {<ErrorType>} <When/why>
 *
 * @usage
 * ```<language>
 * <Minimal working code snippet>
 * ```
 ******************************************************************************************************************/
```

## Comment Block For Classes / Services / Providers
Classes, services, providers, dependency-injection containers, context managers, or equivalents must have a block comment directly above their declaration.
Follow the **General Conventions** for ```@template```, ```@param```, ```@property```, ```@throws```, and ```@usage```.
```javascript
/******************************************************************************************************************
 * <Brief imperative summary of the class/service/provider>
 *
 * @template T - <What T represents / constraint / default if applicable>
 *
 * @property <field_1>  - <Meaning/constraints>
 * @property <field_2>? - <Optional/defaults>
 *
 * @param <ctorOrProps> - <Constructor args or provider props>
 *
 * @throws {<ErrorType>} <When/why>
 *
 * @usage
 * ```<language>
 * <Minimal working example showing instantiation/wrapping/consumption>
 * ```
 ******************************************************************************************************************/
```

### Notes
- Only document constructor parameters in the class block.
- Individual methods inside the class must be documented separately using the **function block format**.
- Do not define a ```@return``` as it is expected that the class or equivalent will be returned itself.

## Comment Block For Types / Interfaces
Types, interfaces, structs, or equivalent schema definitions must have a block comment directly above their declaration.
Follow the **General Conventions** for ```@template```, ```@property```, and ```@usage```.
```javascript
/******************************************************************************************************************
 * <Brief imperative summary of the type/interface and its contracts>
 *
 * @template T - <What T represents / constraint / default if applicable>
 *
 * @property <field_1>    - <Meaning/constraints>
 * @property <nestedObj>  - <Nested description>:
 *   - <nestedField_1>  - <Description>
 *   - <nestedList>?    - <List description>:
 *     + <listElem>       - <List element description>
 *
 * @usage
 * ```<language>
 * <Minimal example constructing/consuming the type/interface>
 * ```
 ******************************************************************************************************************/
```
