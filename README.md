# Books API: Postman Collection with Newman and GitHub Actions

A Postman collection that exercises a REST API for books (create, read, update and delete), run automatically with **Newman** in a **GitHub Actions** pipeline that publishes an HTML report on every push.

![Run Postman API Tests](https://github.com/Naorinr95/Api-collection-gh-actions/actions/workflows/postman-api-tests.yml/badge.svg)

## What is tested

| Request | Method | Check |
|---------|--------|-------|
| Add Book | POST `/books` | Returns 201; request body is built from random pre-request data |
| Get All Books & Randomize | GET `/books` | Returns 200; picks a random book ID and saves it to the environment |
| Get Book by Random ID | GET `/books/{id}` | Returns 200 for the saved ID |
| Update Book | PATCH `/books/{id}` | Returns an accepted status (200 or 403) |
| Delete Book | DELETE `/books/{id}` | Returns an accepted status (200 or 403) |

Each field of the Add Book and Update Book payloads (title, author, genre, year) comes from its own random variable, set by a pre-request script.

## Files

```
Books API Collection_Naorin.json    Postman collection
Books Collection Env_Naorin.json    Postman environment (baseUrl and variables)
.github/workflows/                  CI pipeline
```

## Running locally

```bash
npm install -g newman newman-reporter-htmlextra

newman run "Books API Collection_Naorin.json" \
  -e "Books Collection Env_Naorin.json" \
  -r cli,htmlextra \
  --reporter-htmlextra-export newman/newman-report.html
```

Open `newman/newman-report.html` for the HTML report.

## Continuous integration

On every push to `main`, on every pull request, and on demand, GitHub Actions:

1. Installs Newman and the HTML Extra reporter
2. Runs the collection against the environment file
3. Uploads the HTML report as the `newman-html-report` artifact (even if the run fails)

Open the **Actions** tab, choose a run, and download the artifact at the bottom of the page.

## Notes

- The API under test is a public mock service (`https://fakeapi.extendsclass.com`). It accepts create, update and delete requests but does not save the changes, so these tests check request and response behaviour, not data persistence.
- The collection was originally written against a Glitch-hosted API that has since been shut down, and was moved to this service.

## Author

**Rifat Naorin**, Software QA Engineer

[GitHub](https://github.com/Naorinr95)
