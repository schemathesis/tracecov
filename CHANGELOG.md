# Changelog

## 0.26.1 - 2026-11-22

### :bug: Fixed

- Record `schemathesis fuzz` traffic in the coverage report, labelled `phase:fuzz`.

### :racing_car: Performance

- Up to 4x faster specification loading and 3x less memory for responses sharing schemas.

## 0.26.0 - 2026-11-02

### :rocket: Added

- Robot Framework listener with coverage per suite and test, pabot support, and saved coverage data.
- `label_entries()` in the Python API, listing spec elements some labels reached and others did not.
- JavaScript coverage labels: `labeled()`, `record()` labels, `coveredBy()`, `labelEntries()`, label summaries and thresholds; Professional only.
- `base_path` in the JSON report and the MCP payload.

### :wrench: Changed

- `export_data()` and `merge_data()` replace `export_counts()` and `merge_counts()`, also in JavaScript.
- Reports, APIs and event labels name operations by specification path, without the server base path.
- `operation_report()`, `covered_by()`, `record_error()` and `record_schemathesis_interactions()` take specification paths.

### :bug: Fixed

- Resolve specification paths that repeat the server base path to their own operation.
- List schema errors next to their operation when the specification has a base path.
- Keep the active `labeled()` and per-test labels when `record()` gets explicit `labels`.
- Raise `TypeError` for a single string passed as `labels` to `record()`.
- Count a label repeated within one request once in `label_overflow()`.
- Reject `covered_by()` arguments naming two elements, such as `status` with `location`.
- Raise `TypeError` from `labeled()` for labels that are not strings.
- Drop coverage labels when the Community edition merges coverage data.

### :racing_car: Performance

- Faster recording of requests carrying thousands of labels.

## 0.25.5 - 2026-10-23

### :rocket: Added

- Pre-built wheels for free-threaded Python 3.15 (`3.15t`).

## 0.25.4 - 2026-10-18

### :bug: Fixed

- Record the address the client used in `tracecov proxy --upstream` and `--run` reports and traffic.
- Explain an empty HTML report when the specification has no operations.
- Write Schemathesis hook reports as UTF-8, fixing `UnicodeEncodeError` on Windows.

## 0.25.3 - 2026-10-09

### :rocket: Added

- Coverage of JSON Schemas split into local JSON and YAML files through `$ref`.

### :bug: Fixed

- Stop crashing on string properties with `contentSchema`; report it as an annotation.
- Read `$ref`ed files whose paths are percent-encoded, such as `my%20dir/limit.json`.
- Report `$ref`s with a URI scheme, such as `urn:`, as not followed.

## 0.25.2 - 2026-10-08

### :rocket: Added

- `--suggestions` in `tracecov report` and `tracecov proxy` adds suggested requests to the JSON report.

### :bug: Fixed

- Create missing parent directories when writing reports, traffic files and exported CA certificates.
- Stop requiring invalid values for path parameter patterns like `^[^/]+$`, which no request can send.
- Stop crashing on object keys matching a `propertyNames` `enum`.
- Credit the `string` entry of a `propertyNames` `type` array.

## 0.25.1 - 2026-10-03

### :rocket: Added

- `export_counts()` and `merge_counts()` in the Python API, `exportCounts()` and `mergeCounts()` in JavaScript.

### :bug: Fixed

- Load component schemas with their own `$id` whose `$ref`s point inside themselves.
- Sort operation errors in the HTML report.

### :racing_car: Performance

- Faster pytest-xdist runs whose memory stays flat however much traffic the tests record.
- Smaller worker files and faster merging in the Vitest, Jest and Playwright integrations.

## 0.25.0 - 2026-10-02

### :rocket: Added

#### Coverage and reports

- Each value of a schema's `examples` array counts as an example of its own.
- Why a schema admits no value: the keywords that leave it empty and where they are, in every report and both APIs.
- Unmatched requests section in the HTML, Markdown, text, and JSON reports.
- Constraints no value can satisfy read as inapplicable rather than untested.
- Specification problems section in the HTML, Markdown, text, and JSON reports, and both APIs.
- Report schemas no value can satisfy, such as an `allOf` of conflicting types.
- Report unknown `$schema` and `jsonSchemaDialect` dialects as specification problems, naming the assumed draft.
- Report `$schema` that OpenAPI 2.0 and 3.0 schemas ignore, naming the draft coverage uses.
- Threshold verdict and per-metric minimums in the HTML and Markdown reports.
- Per-metric coverage summary after every Schemathesis run, and `generate_text_summary()` in the Python API.

#### Specifications

- OpenAPI 3.2 support: `querystring` parameters, `additionalOperations`, and `$ref` media types.
- YAML files as targets of external `$ref`s.

#### Coverage labels

- Per-test coverage labels and per-label thresholds in the Python API, pytest plugin, and JSON report.
- `X-TraceCov-Labels` request header labelling traffic in `tracecov proxy`.
- `phase:examples`, `phase:coverage`, `phase:fuzzing` and `phase:stateful` labels on Schemathesis traffic.

#### CLI

- `mcp` command serving coverage to coding agents, and `--upstream` / `--run` recording modes in `proxy`.

#### Python and JavaScript

- `json` format and `--tracecov-report-json-path` in the pytest plugin.
- `unmatched_requests()`, `unmatched_overflow()` and `dropped_requests()` in the Python and JavaScript APIs.
- `CoverageMap.suggestions()` in the Python API, Professional only.
- Context-level `extraHTTPHeaders` recording in the Playwright fixture and `wrapRequest`.

### :bug: Fixed

#### Specification loading and `$ref`s

- Report non-boolean operation `deprecated`, and invalid Encoding Object `style` and `explode`.
- Stop `$ref` cycles in parameter schemas from crashing the process.
- Never fetch a remote `$schema` or `$ref` while loading a specification.
- Read the shared `parameters` of path items reached through `$ref`.
- Measure an operation's other responses beside an invalid status code key or unresolvable `$ref`.
- Follow a root `$schema` or `jsonSchemaDialect` naming a known draft in OpenAPI 3.1+ schemas.
- Apply YAML merge keys (`<<`) in specifications.
- Read an unquoted `openapi: 3.1` or `swagger: 2.0` in YAML specifications as that version.
- Follow chained `$ref`s to parameters, request bodies and responses; report `$ref` loops.
- Resolve external `$ref`s relative to the specification's directory in the CLI, `from_path`, and `fromPath`.
- Measure coverage through `$ref` to an `$anchor` in OpenAPI 3.1 and 3.2 components.
- Load schemas whose `$ref`s point inside an embedded `$id` resource.
- Report `responses` keys outside 100-599, such as `600`, as specification problems.
- Show unresolvable operation-level `$ref`s as written in JavaScript `errors()` and specification problems.
- Name the invalid `responses` key in JavaScript `errors()` and specification problems.
- Read schema paths like `C:\spec.json` or `localhost:8080/spec.json` as files, not URLs.
- Resolve `$ref`s to whole files written without a `#` fragment.

#### Schema keyword coverage

- Expect invalid values for `anyOf` / `oneOf` parameters whose string branch has constraints.
- Validate nested Draft 2019-09 and 2020-12 subschemas in OpenAPI 2.0 and 3.0 schemas.
- Follow a subschema's own `$schema` when deciding which of its keywords count toward coverage.
- Exclude keywords OpenAPI 2.0 and 3.0 do not support, such as `const`, from coverage.
- Stop crashing on `$dynamicRef` and measure the keywords it reaches.
- Count a `false` schema once per checked value in coverage reports.
- Cover `not` only after it both passes and fails.
- Treat a `$ref` to a `false` schema as rejecting every value.
- Treat a `type` list or `anyOf` naming every type but `integer` as accepting any value.
- Report `contentEncoding` and `contentMediaType` as not enforced by validation.
- Count values as valid when a `$ref` sits inside `anyOf`, `oneOf` or `allOf`.
- Accept `null` against a `type` declared beside `nullable: true` or `x-nullable: true`.
- Record coverage for the `float` and `password` formats.
- Report no invalid coverage for a matched option of an array-valued `type`.

#### Parameters and request matching

- Record path parameters whose percent-decoded value is not valid UTF-8.
- Read non-exploded `spaceDelimited` and `pipeDelimited` query parameters holding objects.
- Read array items and form properties behind `$ref` as their referenced type.
- Split exploded `label` path arrays on `.`, reading `.3.4.5` as three items.
- Use the first value when two dot-notation query keys name one array index.
- Keep other query parameters out of exploded free-form object query parameters.
- Read Swagger 2.0 arrays of arrays by each inner array's `collectionFormat`; report unknown ones.
- Match requests under any listed server, server variable `enum` value, or path/operation server.
- Read relative server URLs like `api/v1` as base paths.
- Read object parameter properties, including `additionalProperties` and `$ref`s, as their schema types.
- Read primitive `matrix` and `label` path parameters and `matrix` array items as their schema types.
- Match `/items/` requests to `/items/` operations when the spec also documents `/items`.
- Require an invalid value for object-typed properties of exploded object query parameters.
- Wrong coverage for adjacent path parameters like `/{id}{ext}`.
- Requests to a path key that carries its own query string (e.g. `/current?lat={lat}`) not reaching its operation.
- Path parameter values taken from a template that matched only part of the URL.
- Accept full URLs in `-b/--base-path` and the `base_path` argument.
- Record coverage and schema errors for operations under a base path.
- Crash on requests whose value for a path segment like `{name}-{version}.zip` starts with a non-ASCII character.
- Credit valid CSV values against an OpenAPI 2 array parameter's `enum`, not just its `items`.
- Record path parameter coverage when a static sibling path shadows the recorded operation's template.
- Accept array-level `enum` values on Swagger 2.0 array parameters declared without `items`.

#### Request bodies

- Read repeated urlencoded keys and multipart parts as array elements.
- Ignore Encoding Object `style` and `explode` for OpenAPI 3.0 multipart bodies.
- Apply the request body `encoding` when the body schema is a `$ref`.
- Stop XML bodies nested deeper than 128 elements from crashing the process.
- Reject XML bodies with text or elements outside the root element.
- Read XML bodies in the encoding their byte order mark or declaration names.
- Record request bodies declared under `*/*` or `application/*` media type ranges.
- Accept whitespace before `;` in `Content-Type` headers.
- Count a `type: file` form field once per urlencoded request.
- Match multipart form fields whose names contain quotes or backslashes.
- Record no coverage from malformed multipart bodies; read fields after parts without Content-Disposition.
- Read multipart field values containing the boundary mid-line.
- Credit XML elements and attributes renamed with `xml.name` to their schema properties.
- Treat the string `"null"` as a value in JSON payloads instead of as null.
- Mark scalar `type: string` as fully covered in XML request bodies.
- Resolve entity and character references like `&amp;` and `&#65;` in XML bodies.
- Ignore namespace declarations and undeclared `xml:*`/`xsi:*` attributes in XML bodies; read `xsi:nil` as `null`.
- Record coverage from JSON bodies with a byte order mark or a UTF-16 or UTF-32 `charset`.

#### HAR, Postman and VCR import

- Record each Postman query parameter once, without re-encoding percent-encoded values.
- Record response body coverage from HAR, VCR and Postman traffic.
- Decode base64-encoded `postData` request bodies in HAR 1.3 files.
- Record multipart request bodies given only as `postData.params` in HAR files.
- Recognize Postman v1.0 collections and collections without `_postman_id` in `tracecov report`.
- Record Postman v2.0 requests written as a bare URL string.
- Read VCR and Postman elapsed times written as `0.250` or `2.5e-1` in `tracecov report`.
- Record base64-encoded request bodies from VCR cassettes, including line-wrapped ones.
- Use request start times in coverage timelines recorded from HAR files.
- Record HAR request bodies labelled only by `postData.mimeType`, without a `Content-Type` header.
- Read Postman v1 request times as milliseconds in coverage timelines.
- Name the supported versions when a Postman collection matches none of them.
- Read YAML traffic files, such as VCR cassettes, in `tracecov report`.

#### CLI and `proxy`

- Stop `tracecov proxy` and write its report while clients stay connected or stop reading.
- State the cause of a CLI report file error once.
- Forward HTTP/2 requests to HTTP/1.1-only plain-HTTP servers in `tracecov proxy`.
- Measure response keyword coverage in `proxy`.
- Record HTTPS requests tunnelled through `proxy` over HTTP/1.1 without crashing.
- Answer an aborted request upload with 400 in `proxy`, counting it as dropped.
- Name missed thresholds on stderr when `--quiet` or stdout output hides the summary.

#### Reports

- Keep the measured coverage state of operations with recorded errors in the HTML report.
- Stop calling partially covered parameters and keywords untested in the Markdown report.
- Count operations hidden by `skip_empty`, `--tracecov-skip-empty` and `--coverage-skip-empty` in the text report TOTAL.
- Keep numeric columns in the text report when an operation path exceeds the width.
- Show `No operations to measure.` as the Markdown headline for specs without operations.
- Render names containing `|`, backticks or Markdown syntax intact in Markdown report tables.
- Make "Expand all" / "Collapse all" in the HTML report follow rows opened by hand.
- Show whether the "Deprecated" and "Not measured" filter chips are on in the HTML report.
- List an explicit `default` response under untested operations in the HTML report.
- Style `TRACE` and `QUERY` method badges in the HTML report.
- Allow `QUERY` in the `method` enum of the coverage report JSON Schema.
- Describe response body `keywords` as `violating` or `mixed` only in the coverage report JSON Schema.
- Link every `$ref` in the HTML report to the referenced component's own entry.
- Link `$ref`s to components whose names need percent or JSON-pointer escaping in the HTML report.
- Escape component names shown in the HTML report.
- Keep distinct tags in separate HTML report groups.
- Pass coverage thresholds met exactly, such as 29 of 100 against a 29% minimum.
- Show no percentage for operations without keywords in the HTML report.
- Mark a missed `--fail-under-examples` threshold on the Examples summary row.

#### Python and JavaScript libraries

- Include every seen example in the examples `timeline` of `statistic()`.
- Add missing type hints for the HTML, JSON and Markdown report methods of `CoverageMap`.
- Record `open()` calls and each redirect hop in the Flask `track_client`.
- Default `response` to `None` in `CoverageMap.record()` and `requests.record()`.
- Stop `AsyncClient` requests failing under the hook from `httpx.async_response_hook()`.
- Keep the wrapped method's `__doc__` on clients passed to Django and Flask `track_client()`.
- Name the invalid method and the valid ones in JavaScript `record()` and `recordError()` errors.
- Require `response` and `timestamp` in the `HttpInteraction` type hints.
- Accept `headers=None` in `HttpRequest` and `HttpResponse`.
- Show why coverage failed for an OpenAPI spec in Schemathesis runs.
- Raise `OSError` subclasses naming the file when `from_path` or `record_from_path` cannot read it.
- Raise `TypeError` for a non-`str` request URL or a path of the wrong type.

### :wrench: Changed

- Invalid schema messages name the offending keyword and its value, describing large values by shape.
- Specific error messages naming parameters, allowed values and files; explanatory `proxy` 502 and 429 bodies.
- Exclude paths no request can reach from coverage percentages and `coverage_gaps()`, rather than counting them as zero.
- Alignment, spacing and emphasis throughout the HTML report.
- Color-coded keyword counts and coverage bars for referenced components in the HTML report.
- Unroutable paths move from operation `warnings` to `diagnostics` in the JSON report.
- Optional `method` in JavaScript `errors()` entries.
- Drop and report interactions in `proxy` when recording falls behind.
- Resolve `#/...` references inside externally referenced files against that file.
- Python, JavaScript and test-plugin error messages name the rejected input and accepted values.

### :fire: Removed

- `show_suggestions` argument of `CoverageMap.save_report`.

### :racing_car: Performance

- Faster request forwarding in `proxy`.
- Faster coverage recording for Schemathesis runs and pytest-xdist workers with large response schemas.
- Record requests up to seven times faster.
- Release the GIL while loading specifications, recording traffic and generating reports in the Python API.

