# Esmorga API Collections

Bruno API collection for testing the Esmorga Backend APIs.

## Requirements

- [Bruno](https://www.usebruno.com/)

## Collection

The Bruno collection is located in:

`Esmorga API/`

The collection contains API requests and tests organized by functionality:

- Setup
- Account
- Events
- Event Participation
- Polls
- Users
- Account Lifecycle
- Session

## Environments

The collection includes the following environments:

- LOCAL
- QA
- PROD

Environment files contain the variables required by the collection.

User credentials are managed separately and must not be committed to the repository.

## User roles

The collection supports different user roles:

- USER
- ADMIN

The current user role is obtained dynamically during login and stored in `userRole`.

Tests adapt their expected behavior depending on the authenticated user's role when the API behavior differs between USER and ADMIN.

## Data setup

Some test flows require data to be created before execution.

For these scenarios, an Admin Helper account is used to prepare the required test data without replacing the authentication context of the user under test.

## Running the collection

1. Open the `Esmorga API` collection in Bruno.
2. Select the required environment (`LOCAL`, `QA` or `PROD`).
3. Select the Global Environment containing the credentials for the user to test.
4. Run the collection.

Requests tagged as `manual` are intended for manual execution and should be excluded from automated collection runs.

## Automation

CLI and CI/CD execution will be added separately using Bruno CLI and GitHub Actions.
