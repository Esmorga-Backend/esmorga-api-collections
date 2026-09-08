# Bruno OpenAPI Sync Evaluation

## Objective

Evaluate Bruno OpenAPI Sync to keep the QA API collection aligned with the existing Swagger/OpenAPI specification while preserving QA-specific tests, scripts, assertions and configuration.

## OpenAPI Source

Swagger UI:

`https://qa.api.esmorgaevents.com/swagger#/`

OpenAPI specification:

`https://qa.api.esmorgaevents.com/swagger-json`

The Bruno collection is configured to automatically check the OpenAPI specification for changes.

## Evaluation Results

### Initial OpenAPI Sync

The OpenAPI specification was successfully imported into Bruno.

- 24 endpoints were detected from the OpenAPI specification.
- Endpoints can be grouped using OpenAPI tags.
- Bruno provides a review step before applying changes.
- Individual changes can be accepted or rejected.
- Spec differences can be inspected before synchronization.

### Existing QA Collection

OpenAPI Sync was also configured against the existing `Esmorga API` QA collection.

Bruno successfully matched most existing requests with their corresponding OpenAPI operations.

The evaluation showed that differences reported by OpenAPI Sync do not necessarily represent API contract changes. Some differences are caused by QA-specific configuration, for example:

- Dynamic authentication using `{{accessToken}}`.
- Dynamic request body values used by automated tests.
- Test data and setup requests.
- Manual QA requests.
- Example values defined in the OpenAPI specification.

For this reason, OpenAPI updates should always be reviewed before being applied.

`Accept All` should not be used without reviewing the proposed changes.

### Path Parameters

Two existing requests were initially detected as new OpenAPI endpoints:

- `GET /v1/events/:eventId/users`
- `POST /v1/polls/:pollId/vote`

The QA collection originally used variables directly in the URL:

`/v1/events/{{eventId}}/users`

`/v1/polls/{{pollId}}/vote`

After defining them as Bruno path parameters:

`/v1/events/:eventId/users`

with:

`eventId = {{eventId}}`

and:

`/v1/polls/:pollId/vote`

with:

`pollId = {{pollId}}`

Bruno correctly matched the requests with the OpenAPI specification.

This structure should therefore be preferred for dynamic path parameters.

### Preservation of QA Logic

According to the Bruno OpenAPI Sync review interface:

> Parameters, headers, body and auth are updated. Tests, scripts and assertions are always preserved.

This behavior was also verified during the evaluation using the existing `Esmorga API` collection.

After applying OpenAPI updates to existing requests:

- Request parameters, headers, authentication and body definitions were synchronized with the OpenAPI specification where the update was accepted.
- Existing QA tests were verified to remain unchanged after synchronization. Bruno also indicates that scripts and assertions are preserved during synchronization.
- Requests where `Keep Current` was selected retained their QA-specific configuration.

This confirms that QA automation logic can remain in the Bruno collection while OpenAPI Sync is used to synchronize API contract changes.


### Post-Sync Status

After reviewing and synchronizing the detected OpenAPI differences, Bruno reported:

- 37 requests in the QA collection.
- 33 requests in sync with the OpenAPI specification.
- 4 requests intentionally changed in the QA collection.
- 0 pending specification updates.
- 24 endpoints in the OpenAPI specification.

The remaining collection changes represent intentional QA-specific differences and do not indicate pending OpenAPI updates.

### Limitations and Considerations

The following considerations were identified during the evaluation:

- OpenAPI Sync should not be treated as an automatic replacement mechanism for the QA collection.
- Differences may represent QA-specific configuration rather than API contract changes.
- Each synchronization should be reviewed before applying updates.
- `Accept All` should be avoided unless all detected changes have been reviewed.
- Requests intentionally kept different from the specification remain visible under `Collection Changes`.
- The Bruno plan used during this evaluation displayed a limit of 5 OpenAPI synchronizations per month.
- The synchronization limit may depend on the Bruno plan and should be considered before introducing OpenAPI Sync into a frequent automated workflow.

## Recommended Workflow

The recommended workflow is:

1. Backend updates the Swagger/OpenAPI specification.
2. Bruno checks the configured OpenAPI source for updates.
3. QA reviews the detected differences.
4. QA inspects the Spec Diff before applying changes.
5. Relevant API contract changes are accepted.
6. QA-specific configuration is kept when appropriate.
7. The Bruno collection is executed to verify existing tests.
8. The resulting Git diff is reviewed.
9. Changes are committed through the normal feature branch and pull request workflow.

Flow:

`Swagger/OpenAPI → Detect Changes → QA Review → Selective Sync → Test Execution → Git Review → Pull Request`

## Conclusion

Bruno OpenAPI Sync is suitable as a support tool for keeping the QA API collection aligned with the backend API specification.

The evaluation confirmed that OpenAPI contract changes can be detected, reviewed and selectively applied to the existing QA collection while preserving QA-specific tests, scripts and assertions.

It should be used as a reviewed synchronization mechanism rather than automatically replacing the QA collection with the OpenAPI-generated version.

The existing `Esmorga API` collection remains the QA source for tests, scripts, test data setup and automation-specific configuration, while Swagger/OpenAPI acts as the API contract used to detect changes.

Intentional differences between the QA collection and the OpenAPI specification are acceptable and can be maintained using `Keep Current`.

Based on the evaluation, the recommended approach is:

`Swagger/OpenAPI → Detect Changes → QA Review → Selective Sync → Execute Tests → Git Review → Pull Request`