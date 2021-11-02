![portman-hero](https://user-images.githubusercontent.com/1112129/125833512-c32359d8-af27-495b-8211-744c504146b2.png)

<p align="center">
  <a href="https://www.npmjs.com/package/@apideck/portman"><img src="https://img.shields.io/npm/v/@apideck/portman.svg" alt="Total Downloads"></a>
  <a href="https://www.npmjs.com/package/@apideck/portman"><img src="https://img.shields.io/npm/dw/@apideck/portman.svg" alt="Latest Stable Version"></a>
</p>


# Portman 👨🏽‍🚀

Port OpenAPI Spec to Postman Collection, with contract & variation tests included!

Portman leverages OpenAPI documents, with all its defined API request/response properties, to power your Postman collection.
Let Portman do all the work and inject contract & variation tests with a minimum of configuration.
Customize the Postman requests & variables with a wide range of options to assign & overwrite variables.

## Why use Portman?

Convert your OpenAPI spec to Postman, generate contract & variation tests, upload the Postman collection & run the tests through Newman.
Include the Portman CLI as part of an automated process for injecting the power of Portman directly into your CI/CD pipeline.

[Read the full blog post](https://blog.apideck.com/announcing-portman)

## Features

With Portman, you can:

- [x] Convert an OpenAPI document to a Postman collection
  - [x] Support for OpenAPI 3.0
  - [ ] Support for OpenAPI 3.1
- Extend the Postman collection with capabilities
  - [x] Assign collection variables
  - [x] Inject Postman Contract Tests
  - [x] Inject Postman Variation Tests
  - [x] Inject Postman Integration Tests
  - [x] Inject Postman with Pre-request scripts on a collection or operation level
  - [x] Modify Postman requests
- [x] Upload the Postman collection to your Postman app
- [x] Test the Postman collection with Newman
- [x] Manage everything in config file for easy local or CI/CD usage

## Getting started

1. Install Portman
2. Initialize Portman CLI configuration by running: `$ portman --init`

OR

1. Install Portman
2. Copy `.env.example` to `.env` and add environment variables you need available to your collection
3. Copy/rename and customize each of the \_\_\_\_.default.json config files in the root directory to suit your needs
4. Start converting your OpenAPI document to Postman

All configuration options to convert from OpenAPI to Postman can be found in the [openapi-to-postman](https://github.com/postmanlabs/openapi-to-postman/blob/develop/OPTIONS.md) package documentation.
All configuration options to filter flags/tags/methods/operations/... from OpenAPI can be found in the [openapi-format](https://github.com/thim81/openapi-format#openapi-filter-options) package documentation.

## Installation

### Local Installation (recommended)

You can add the Portman CLI to the `node_modules` by using:

```shell
$ npm install --save @apideck/portman
```

or using yarn:

```shell
$ yarn add @apideck/portman
```

Note that this will require you to run the Portman CLI with `npx @apideck/portman -l your-openapi-file.yaml` or, if
you are using an older version of npm, `./node_modules/.bin/Portman -l your-openapi-file.yaml`.

### Global Installation

```shell
$ npm install -g @apideck/portman
```

### NPX usage

To execute the CLI without installing it via npm, use the npx method.

```shell
$ npx @apideck/portman -l your-openapi-file.yaml
```

## CLI Usage

```
Usage: -u <url> -l <local> -b <baseUrl> -t <includeTests>

Options:
      --help                 Show help                                                                        [boolean]
      --version              Show version number                                                              [boolean]
  -u, --url                  URL of OAS to port to Postman collection                                         [string]
  -l, --local                Use local OAS to port to Postman collection                                      [string]
  -b, --baseUrl              Override spec baseUrl to use in Postman                                          [string]
  -o, --output               Write the Postman collection to an output file                                   [string]
  --oaOutput                 Write the (filtered) OpenAPI file to an output file                              [string]
  -n, --runNewman            Run Newman on newly created collection                                           [boolean]
  --newmanRunOptions         JSON stringified object to pass options for configuring Newman                   [string]
  --newmanOptionsFile        Path to Newman options file to pass options for configuring Newman               [string]
  -d, --newmanIterationData  Iteration data to run Newman with newly created collection                       [string]
  --localPostman             Use local Postman collection, skips OpenAPI conversion                           [string]
  --syncPostman              Upload generated collection to Postman (default: false)                          [boolean]
  -p, --postmanUid           Postman collection UID to upload with the generated Postman collection           [string]
  --postmanWorkspaceName     Postman Workspace name to target the upload of the generated Postman collection  [string]
  -t, --includeTests         Inject Portman test suite (default: true)                                        [boolean]
  --bundleContractTests      Bundle Portman contract tests in a separate folder in Postman (default: false)   [boolean]
  -c, --portmanConfigFile    Path to Portman settings config file (portman-config.json)                       [string]
  -s, --postmanConfigFile    Path to openapi-to-postman config file (postman-config.json)                     [string]
  --filterFile               Path to openapi-format config file (oas-format-filter.json)                      [string]
  --envFile                  Path to the .env file to inject environment variables                            [string]
  --cliOptionsFile           Path to Portman CLI options file                                                 [string]
  --init                     Configure Portman CLI options in an interactive manner                           [string]
```

### Environment variables as Postman variables

Portman uses `dotenv` to not only access variables for functionality, but you can also add environment variables that you'd like declared within your Postman environment.
Simply prefix any variable name with `PORTMAN_`, and it will be available for use in your Postman collection as the camel-cased equivalent. For example:

```
PORTMAN_CONSUMER_ID=test_user_id
```

will be available in your collection or tests by referencing:

```
{{consumerId}}
```

It is possible to set a spec-specific `.env` file, that lives next to your config files. The path can be passed in via `envFile` cli option.
This is useful if you have Portman managing multiple specs that have unique environment requirements.

By default, Portman will leverage any ENVIRONMENT variable that is defined that starts with `PORTMAN_`.

### CLI Options

- Initialize Portman CLI configuration:

```
portman --init
```

The `init` option will help you to configure the cliConfig options and put the default config, env file in place to kick-start the usage of Portman.

- Pass in the remotely hosted spec:

```
portman -u https://specs.apideck.com/crm.yml
```

- Overwrite the baseUrl in spec and run Newman.

```
portman -u https://specs.apideck.com/crm.yml -b http://localhost:3050 -n true
```

- Path pass to a local data file for Newman to use for iterations.

```
portman -u https://specs.apideck.com/crm.yml -b http://localhost:3050 -n true -d ./tmp/newman/data/crm.json
```

- Pass the path to a local spec (useful when updating your specs) and output Postman collection locally

```
portman -l ./tmp/specs/crm.yml -o ./tmp/specs/crm.postman.json
```

- Skip tests and just generate collection.

```
portman -l ./tmp/specs/crm.yml -t false
```

- Filter OpenAPI and generate collection.

```
portman -u https://specs.apideck.com/crm.yml --filterFile examples/cli-filtering/oas-format-filter.json
```

For more details, review the [cli-filtering example](https://github.com/apideck-libraries/portman/tree/main/examples/cli-filtering).

- Upload newly generated collection to Postman, which will upsert the collection, based on the collection name

```
portman -l ./tmp/specs/crm.yml --syncPostman true
```

Upload newly generated collection to Postman using the collection UID to overwrite the existing.

```
portman -l ./tmp/specs/crm.yml --syncPostman true -p 9601963a-53ff-4aaa-92a0-2e70a8a2a748
```

- Pass custom paths for config files

All configuration options to convert from OpenAPI to Postman can be on the [openapi-to-postman](https://github.com/postmanlabs/openapi-to-postman/blob/develop/OPTIONS.md) package documentation.
Portman provides a default openapi-to-postman configuration [postman-config.default.json](postman-config.default.json), which will be used if no custom config `--postmanConfigFile` is passed.

Portman configuration file in JSON format:
```
portman -u https://specs.apideck.com/crm.yml -c ./tmp/crm/portman-config.json -s ./common/postman-config.json
```

Portman configuration file in YAML format:
```
portman -u https://specs.apideck.com/crm.yml -c ./tmp/crm/portman-config.yaml -s ./common/postman-config.json
```

- Pass all CLI options as JSON file

All the CLI options can be managed in a separate configuration file and passed along to the portman command. This will
make configuration easier, especially in CI/CD implementations.

```
portman --cliOptionsFile ./examples/cli-options/portman-cli-options.json
```

All the available Portman CLI options can be used in the config file.
By passing the CLI options as parameter, you can overwrite the defined CLI options defined in the file.

For more details, review the [cli-options example](https://github.com/apideck-libraries/portman/tree/main/examples/cli-options).

- Run Newman with Newman options

All [Newman configuration options](https://learning.postman.com/docs/running-collections/using-newman-cli/command-line-integration-with-newman/#options) to run Newman can be passed along through Portman.

```
portman -u https://specs.apideck.com/crm.yml -c ./tmp/crm/portman-config.json --runNewman --newmanOptionsFile ./tmp/crm/newman-options.json
```

For more details, review the [cli-options example](https://github.com/apideck-libraries/portman/tree/main/examples/cli-options).


### Output

Without specifying the output location, your generated Postman Collection is written to `./tmp/converted/${specName}.json` if you are manually importing to Postman or need to inspect for debugging.

By using `-o` or `--output` parameter, you can define the location where the Postman collection will be written.

```
portman -l ./tmp/specs/crm.yml -o ./tmp/specs/crm.Postman.json
```

### NOTE:

Newman is set to ignore redirects to allow for testing redirect response codes. If you are running collections within Postman UI, you'll need to ensure Postman is set to the same, or your redirect tests will fail.

Postman > Preferences > Automatically follow redirects > OFF

## Portman settings

The Portman settings consist out of multiple parts:

- **version** : which refers to the JSON Portman configuration version.
- **tests** : which refers to the definitions for the generated contract & variance tests.
  - **contractTests** : refers to the options to enabled autogenerated contract tests.
  - **contentTests** : refers to the additional Postman tests that check the content.
  - **variationTests** : refers to the options to define variation tests.
  - **integrationTests** : refers to the options to define integration tests.
  - **extendTests** : refers to the custom additions of manually created Postman tests.
- **assignVariables** : which refers to setting Postman collection variables for easier automation.
- **overwrites** : which refers to the custom additions/modifications of the OpenAPI/Postman request data.
- **operationPreRequestScripts** : which refers to injecting Postman Pre-request Scripts for requests.
- **globals** : which refers to the customization that applies for the whole Postman collection.

### Portman targeting

It is possible to inject Postman tests and pre-register scripts, assign variables and overwrite query params, headers, request body data with values.

To be able to do this very specifically, there are options to define the targets:

- **openApiOperationId (String)** : References to the OpenAPI operationId, example: `leadsAll`
- **openApiOperationIds (Array)** : References to an array of OpenAPI operationIds, example: `['leadsAll', 'companiesAll', 'contactsAll']`
- **openApiOperation (String)** : References to a combination of the OpenAPI method & path, example: `GET::/crm/leads`

- **excludeForOperations (Array)** : References to OpenAPI operations that will be skipped for targeting. It supports both the `openApiOperationId` and `openApiOperation` format, example: `["leadsAdd", "GET::/crm/leads/{id}"]`

An `openApiOperationId` is an optional property. To offer support for OpenAPI documents that don't have operationIds, we
have added the `openApiOperation` definition, which is the unique combination of the OpenAPI method & path, with a `::`
separator symbol. The targeting option `excludeForOperations` is really useful when using wildcards, to allow exclusions from the wildcard.

This will allow targeting for very specific OpenAPI items.

To facilitate managing the filtering, we have included wildcard options for the `openApiOperation` option, supporting
the methods & path definitions.

REMARK: Be sure to put quotes around the target definition.

- **Strict matching** example: `"openApiOperation": "GET::/crm/leads",`
  This will target only the "GET" method and the specific path "/pets"

- **Method wildcard matching** example: `"openApiOperation": "*::/crm/leads",`
  This will target all methods ('get', 'put', 'post', 'delete', 'options', 'head', 'patch', 'trace') and the specific
  path "/pets"

- **Path wildcard matching** example: `"openApiOperation": "GET::/crm/*"`
  This will target only the "GET" method and any path matching any folder behind the "/pets", like "/pets/123" and
  "/pets/123/buy".

- **Method & Path wildcard matching** example: `"openApiOperation": "*::/crm/*",`
  A combination of wildcards for the method and path parts is even possible.

### Portman - `tests` properties

The Portman `tests` is where you would define the tests that would be applicable and automatically generated by Portman, based on the OpenAPI document.
The contract tests are grouped in an array of `contractTests`.

#### contractTests options

- **openApiOperationId (String)** : References to the OpenAPI operationId. (example: `leadsAll`)
- **openApiOperationIds (Array)** : References to an array of OpenAPI operationIds, example: `['leadsAll', 'companiesAll', 'contactsAll']`
- **openApiOperation (String)** : References to a combination of the OpenAPI method & path (example: `GET::/crm/leads`)
- **excludeForOperations (Array | optional)** : References to OpenAPI operations that will be skipped for targeting, example: `["leadsAdd", "GET::/crm/leads/{id}"]`

- **statusSuccess (Boolean)** : Adds the test if the response of the Postman request returned a 2xx
- **statusCode (Boolean, HTTP code)** : Adds the test if the response of the Postman request return a specific status code.
- **responseTime (Boolean)** : Adds the test to verify if the response of the Postman request is returned within a number of ms.
- **contentType (Boolean)** : Adds the test if the response header is matching the expected content-type defined in the OpenAPI spec.
- **jsonBody (Boolean)** : Adds the test if the response body is matching the expected content-type defined in the OpenAPI spec.
- **schemaValidation (Boolean)** : Adds the test if the response body is matching the JSON schema defined in the OpenAPI spec. The JSON schema is inserted inline in the Postman test.
- **headersPresent (Boolean)** : Adds the test to verify if the Postman response header has the header names present, like defined in the OpenAPI spec.

For more details, review the [contract-tests example](https://github.com/apideck-libraries/portman/tree/main/examples/testsuite-contract-tests).

#### variationTests options

- **openApiOperationId (String)** : References to the OpenAPI operationId for which a variation will be created. (example: `leadsAll`)
- **openApiOperationIds (Array)** : References to an array of OpenAPI operationIds, example: `['leadsAll', 'companiesAll', 'contactsAll']`
- **openApiOperation (String)** : References to a combination of the OpenAPI method & path for which a variation will be created. (example: `GET::/crm/leads`)
- **excludeForOperations (Array | optional)** : References to OpenAPI operations that will be skipped for targeting, example: `["leadsAdd", "GET::/crm/leads/{id}"]`
- **openApiResponse (String | optional)** : References to the OpenAPI response object code/name for which a variation will be created. (example: `"404"`). If not defined, the 1st response object from OpenAPI will be taken as expected response. If the configured `openApiResponse` code is not defined in the OpenAPI document, Portman will not generate a variation for the targeted operations. 

- **tests** : which refers to the definitions for the generated contract & variance tests for the variation.
  - **contractTests** : refers to the options to enabled autogenerated contract tests for the variation.
  - **contentTests** : refers to the additional Postman tests that check the content for the variation.
  - **extendTests** : refers to the custom additions of manual created Postman tests to be included in the variation.
- **assignVariables** : This refers to setting Postman collection variables that are assigned based on variation.
- **overwrites** : which refers to the custom additions/modifications of the OpenAPI/Postman request data, specifically for the variation.

For more details, review the [content-variation example](https://github.com/apideck-libraries/portman/tree/main/examples/testsuite-variation-tests).

#### integrationTests options

- **name (String)** : As Integration tests will normally contain multiple operations, this is the folder name that will be generated in the Integration Tests folder in your Postman collection.
- **operations (Array)** : Array of operations to be performed

### Portman - `contentTests` properties

Content tests will validate if the response property values will match the expected defined values.
While the Portman `tests` verify the "contract" of the API, the `contentTests` will verify the content of the API.

#### contentTests options

- **openApiOperationId (String)** : References to the OpenAPI operationId. (example: `leadsAll`)
- **openApiOperationIds (Array)** : References to an array of OpenAPI operationIds, example: `['leadsAll', 'companiesAll', 'contactsAll']`
- **openApiOperation (String)** : References to a combination of the OpenAPI method & path (example: `GET::/crm/leads`)
- **excludeForOperations (Array | optional)** : References to OpenAPI operations that will be skipped for targeting, example: `["leadsAdd", "GET::/crm/leads/{id}"]`

- **responseBodyTests (Array)** : Array of key/value pairs of properties & values in the Postman response body.
  - **key (String)** : The key that will be targeted in the response body to check if it exists.
  - **value (String)** : The value that will be used to check if the value in the response body property matches.
  - **contains (String)** : The value that will be used to check if the value is present in the value of the response body property.
  - **length (Number)** : The number that will be used to check if the value of the response body property has a length of the defined number of characters.

- **responseHeaderTests (Array)** : Array of key/value pairs of properties & values in the Postman response header.
  - **key (String)** : The header name that will be targeted in the response header to check if it exists.
  - **value (String)** : The value that will be used to check if the value in the response header matches.
  - **contains (String)** : The value that will be used to check if the value is present in the value of the response header.
  - **length (Number)** : The number that will be used to check if the value of the response header has a length of the defined number of characters.

For more details, review the [content-tests example](https://github.com/apideck-libraries/portman/tree/main/examples/testsuite-content-tests).

### Portman - `extendTests` properties

When you need to add additional tests or overwrite the Portman-generated test, you can use the `extendTests` to define the raw Postman tests.
Anything added in the `tests` array will be added to the Postman test scripts.

#### extendTests options

- **openApiOperationId (String)** : References to the OpenAPI operationId. (example: `leadsAll`)
- **openApiOperationIds (Array)** : References to an array of OpenAPI operationIds, example: `['leadsAll', 'companiesAll', 'contactsAll']`
- **openApiOperation (String)** : References to a combination of the OpenAPI method & path (example: `GET::/crm/leads`)
- **excludeForOperations (Array | optional)** : References to OpenAPI operations that will be skipped for targeting, example: `["leadsAdd", "GET::/crm/leads/{id}"]`

- **tests (Array)** : Array of additional Postman test scripts.
- **overwrite (Boolean true/false | Default: false)** : Resets all generateTests and overwrites them with the defined tests from
  the `tests` array.
- **append (Boolean true/false | Default: true)** : Place the tests after (append) or before (prepend) all generated tests.

<hr>

### Portman - `assignVariables` properties

The "assignVariables" allows you to set Postman collection variables for easier automation.

#### assignVariables options

- **openApiOperationId (String)** : Reference to the OpenAPI operationId for which the Postman pm.collectionVariables will be set. (example: `leadsAll`)
- **openApiOperationIds (Array)** : References to an array of OpenAPI operationIds, for which the Postman pm.collectionVariables will be set. example: `['leadsAll', 'companiesAll', 'contactsAll']`
- **openApiOperation (String)** : Reference to the combination of the OpenAPI method & path, for which the Postman pm.collectionVariables will be set. (example: `GET::/crm/leads`)
- **excludeForOperations (Array | optional)** : References to OpenAPI operations that will be skipped for targeting, example: `["leadsAdd", "GET::/crm/leads/{id}"]`

- **collectionVariables (Array)** : Array of key/value pairs to set the Postman collection variables.
  - **responseBodyProp (String)** : The property for which the value will be taken from the response body and set the value as the pm.collectionVariables value.
  - **responseHeaderProp (String)** : The property for which the value will be taken from the response header and set the value as the pm.collectionVariables value.
  - **requestBodyProp (String)** : The property for which the value will be taken from the request body and set the value as the pm.collectionVariables value.
  - **value (String)** : The defined value that will be set as the pm.collectionVariables value.
  - **name (string OPTIONAL | Default: openApiOperationId.responseProp)** : The name that will be used to overwrite the default generated variable name

For more details, review the [assign-variables example](https://github.com/apideck-libraries/portman/tree/main/examples/testsuite-assign-variables).

<hr>

### Portman - `overwrites` properties

To facilitate automation, you might want to modify properties with "randomized" or specific values. The overwrites are mapped based on the OpenAPI operationId or OpenAPI Operation reference.

#### overwrites options

- **openApiOperationId (String)** : Reference to the OpenAPI operationId for which the Postman request will be overwritten or extended. (example: `leadsAll`)
- **openApiOperationIds (Array)** : References to an array of OpenAPI operationIds, for which the Postman request will be overwritten or extended (example: `['leadsAll', 'companiesAll', 'contactsAll']`)
- **openApiOperation (String)** : Reference to combination of the OpenAPI method & path, for which the Postman request will be overwritten or extended (example: `GET::/crm/leads`)
- **excludeForOperations (Array | optional)** : References to OpenAPI operations that will be skipped for targeting. (example: `["leadsAdd", "GET::/crm/leads/{id}"]`)

- **overwriteRequestQueryParams (Array)** :

  Array of key/value pairs to overwrite in the Postman Request Query params.

  - **key (String)** : The key that will be targeted in the request Query Param to overwrite/extend.
  - **value (String)** : The value that will be used to overwrite/extend the value in the request Query Param OR use the [Postman Dynamic variables](https://learning.Postman.com/docs/writing-scripts/script-references/variables-list/) to use dynamic values like `{{$guid}}` or `{{$randomInt}}`.
  - **overwrite (Boolean true/false | Default: true)** : Overwrites the request query param value OR attach the value to the original request query param value.
  - **disable (Boolean true/false | Default: false)** : Disables the request query param in Postman.
  - **remove (Boolean true/false | Default: false)** : Removes the targeted request query param from Postman.
  - **insert (Boolean true/false | Default: true)** : Insert additional the request query param in Postman that are not present in OpenAPI.
  - **description (String)** : Overwrites the request query param description in Postman.

- **overwriteRequestPathVariables (Array)** :

  Array of key/value pairs to overwrite in the Postman Request Path Variables.

  - **key (String)** : The key that will be targeted in the request Path variables to overwrite/extend.
  - **value (String)** : The value that will be used to overwrite/extend the value in the request path variable OR use the [Postman Dynamic variables](https://learning.Postman.com/docs/writing-scripts/script-references/variables-list/) to use dynamic values like `{{$guid}}` or `{{$randomInt}}`.
  - **overwrite (Boolean true/false | Default: true)** : Overwrites the request path variable value OR attaches the value to the original request Path variable value.
  - **remove (Boolean true/false | Default: false)** : Removes the request path variable.

- **overwriteRequestHeaders (Array)** :

  Array of key/value pairs to overwrite in the Postman Request Headers.

  - **key (String)** : The key that will be targeted in the request Headers to overwrite/extend.
  - **value (String)** : The value that will be used to overwrite/extend the value in the request headers OR use the [Postman Dynamic variables](https://learning.Postman.com/docs/writing-scripts/script-references/variables-list/) to use dynamic values like `{{$guid}}` or `{{$randomInt}}`.
  - **overwrite (Boolean true/false | Default: true)** : Overwrites the request header value OR attaches the value to the original request header value.
  - **remove (Boolean true/false | Default: false)** : Removes the targeted request headers from Postman.
  - **insert (Boolean true/false | Default: true)** : Insert additional the request headers in Postman that are not present in OpenAPI.
  - **description (String)** : Overwrites the request header description in Postman.

- **overwriteRequestBody (Array)** :

  Array of key/value pairs to overwrite in the Postman Request Body.

  - **key (String)** : The key that will be targeted in the request body to overwrite/extend.
  - **value (String)** : The value that will be used to overwrite/extend the key in the request body OR use the [Postman Dynamic variables](https://learning.Postman.com/docs/writing-scripts/script-references/variables-list/) to use dynamic values like `{{$guid}}` or `{{$randomInt}}`.
  - **overwrite (Boolean true/false | Default: true)** : Overwrites the request body value OR attaches the value to the original request body value.
  - **remove (Boolean true/false | Default: false)** : Removes the request body property, including the value.

- **overwriteRequestSecurity (Object)** :

  A Postman RequestAuthDefinition object that will be applied to the request.

For more details, review the [overwrites example](https://github.com/apideck-libraries/portman/tree/main/examples/testsuite-overwrites).

<hr>

### Portman - `operationPreRequestScripts` properties

The `operationPreRequestScripts` configuration will inject pre-request scripts in the Postman collection, on request level.
Postman executes pre-request scripts before a request runs. If you want to set the Postman Collection pre-request scripts on the collection level, you can use the `globals` > `collectionPreRequestScripts` configuration.
The `operationPreRequestScripts` is inserted on the request level.

#### operationPreRequestScripts options

- **openApiOperationId (String)** : Reference to the OpenAPI operationId on which the "Pre-request Scripts" will be inserted. (example: `leadsAll`)
- **openApiOperationIds (Array)** : References to an array of OpenAPI operationIds, for which the "Pre-request Scripts" will be inserted (example: `['leadsAll', 'companiesAll', 'contactsAll']`
- **openApiOperation (String)** : Reference to combination of the OpenAPI method & path, for which the "Pre-request Scripts" will be inserted (example: `GET::/crm/leads`)
- **excludeForOperations (Array | optional)** : References to OpenAPI operations that will be skipped for targeting. (example: `["leadsAdd", "GET::/crm/leads/{id}"]`)

- **scripts (Array)** : Array of scripts that will be injected as Postman Pre-request Scripts on request level, that will be executed before the targeted requests in this collection.

<hr>

### Portman - `globals` property

The configuration defined in the `globals` will be executed on the full Postman collection. This is handy if you need to do mass replacements of variables or specific words/keys/values in the full collection that cannot be overwritten per request.

#### globals options

- **collectionPreRequestScripts** : Array of scripts that will be injected as Postman Collection Pre-request Scripts that will execute before every request in this collection.
- **keyValueReplacements** : A map of parameter key names that will have their values replaced with the provided Postman variables.
- **valueReplacements** : A map of values that will have their values replaced with the provided values.
- **rawReplacements** : Consider this a "search & replace" utility, that will search a string/object/... and replace it with another string/object/...
  This is very useful to replace data from the OpenAPI specification, before it is used in the Portman test automation generation.
- **portmanReplacements** : The "search & replace" utility right before the final Postman file is written, that will search a string/object/... and replace it with another string/object/...
  This is practical to replace any data from the generated Portman collection, before it is used in Postman / Newman test execution.
- **orderOfOperations** : The `orderOfOperations` is a list of OpenAPI operations, which is used by Portman to sort the Postman requests in the desired order, in their folder. Items that are **not** defined in the `orderOfOperations` list will remain at their current order.
- **securityOverwrites** : Provide a overwrite of the OpenAPI Security Scheme Object (supported types: "apiKey", "http basic auth", "http bearer token")

The security overwrites provides a number of security types:

- **apiKey**: The API key auth will send a key-value pair to the API either in the request headers or query parameters.
  - **value (String)** : The value that will be inserted as the Postman apiKey value. It can be a plain value or a Postman variable.
  - **key (String | optional)** : The "key" value that will be inserted in the Postman apiKey key field. It can be a plain value or a Postman variable.
  - **in (String | optional)** : The "in" value that defines where the Api Key will be added in the Postman request Header or Query params. Postman supports `header` for "Header" or `query` for "Query Params".

```json
"securityOverwrites": {
      "apiKey": {
        "value": "{{apiKey}}"
      }
    }
```

- **bearer**: The bearer tokens allow requests to authenticate using an access key, such as a JSON Web Token (JWT).
  - **token (String)** : The "token" that will be inserted as the Postman bearer token value. It can be a plain value or a Postman variable.

```json
"securityOverwrites": {
      "bearer": {
        "token": "{{bearerToken}}"
      }
    }
```

- **basic**: Basic authentication involves sending a verified username and password with your request.
  - **username (String)** : The username that will be inserted as the basic authentication username value
  - **password (String)** : The password that will be inserted as the basic authentication password value

```json
"securityOverwrites": {
      "basic": {
        "username": "{{username}}",
        "password": "{{password}}",
      }
    }
```

For more details on the `globals` configuration options , review the [globals example](https://github.com/apideck-libraries/portman/tree/main/examples/portman-globals) and [ordering example](https://github.com/apideck-libraries/portman/tree/main/examples/postman-ordering)

<hr>

## Configure automatic upload to Postman App

> REMARK: Portman does **not** require you to have a Postman account.

In case you want to sync the generated Postman collection with the Postman app (`portman --syncPostman`), you would need a Postman account since Portman leverages the Postman API to sync the collection.

This can be a "free" Postman account or any of the paid [Postman plans](https://www.postman.com/pricing/).

The generated Postman collection can always be [imported manually](https://learning.postman.com/docs/getting-started/importing-and-exporting-data/#importing-data-into-postman), without a Postman account.

To enable automatic uploads of the generated Postman collection through Portman, follow these steps:

1. Get your Postman API key

![Documentation Pipeline](https://raw.githubusercontent.com/apideck-libraries/portman/main/docs/img/postman-automation-0.png)

![Documentation Pipeline](https://raw.githubusercontent.com/apideck-libraries/portman/main/docs/img/postman-automation-1.png)

![Documentation Pipeline](https://raw.githubusercontent.com/apideck-libraries/portman/main/docs/img/postman-automation-2.png)

2. Goto the root folder of your project

3. Copy [env-postman-app-example](./.env-postman-app.example) as `.env` in the root folder of your project

4. Enter your Postman API key in a local `.env` file, as `POSTMAN_API_KEY=[replace with Postman api key]`

Next to the Postman API key, you can also pass along the Postman Workspace name & the specific Postman Collection UID.

Supported Postman API .ENV variables:
- **POSTMAN_API_KEY** : Postman API key
- **POSTMAN_WORKSPACE_NAME** : Postman Workspace name to target the upload of the generated Postman collection
- **POSTMAN_COLLECTION_UID** : Postman collection UID to upload with the generated Postman collection

The `POSTMAN_WORKSPACE_NAME` & `POSTMAN_COLLECTION_UID` variables can also be set as CLI Options `--postmanWorkspaceName` & `--postmanUid` , which will overrule the variables defined in the .ENV file. 

> **RECOMMENDATION**: Do not commit the `.env` file in any versioning system like GIT if it contains confidential credentials.

# Credits

Portman started as a PR on the handy [openapi-to-postman](https://github.com/postmanlabs/openapi-to-postman) package to generate basic Postman tests from the OpenAPI specification.

[Apideck](https://www.apideck.com/) immediately saw the PR's value and collaborated with the original author, [Tim Haselaars](https://github.com/thim81), to adopt the functionality and extend the options & tooling to create "Portman".

The goal of Portman is to drive API automation by 'porting' a static OpenAPI document to a dynamic Postman collection that includes a powerful testing suite with variable requests, bodies and more. All this while being easy to configure & ready to use.

Portman is a valuable tool in any OpenAPI workflow, for local development or as part of a CI/CD automation pipeline.

Credits for this package for the hard work of [Nick Lloyd](https://github.com/nicklloyd) and [Tim Haselaars](https://github.com/thim81).

# Future ideas

- [ ] Make Postman security dynamic
- [ ] add task to initialize config files
- [ ] add interactive cli prompts
