# AI Software Engineer Assignment (TS)

This project contains a minimal HTTP client implementation with tests that reproduce and verify a bug fix in OAuth token handling.

---

## Prerequisites

- Node.js (v20+ recommended)
- npm
- Docker (optional, for containerized test runs)

---

## Install dependencies (local)

Dependencies are pinned to exact versions for reproducibility.

```bash
npm install
```

---

## Run tests locally

```bash
npm test
```

This runs the Vitest test suite in a clean Node environment.

---

## Run tests with Docker

Build the container:

```bash
docker build -t ai-assignment .
```

Run tests inside Docker:

```bash
docker run --rm ai-assignment
```

The container installs dependencies and executes the test suite automatically.

---

## Project structure

```
src/        → application code
tests/      → test suite
Dockerfile  → CI-style test container
Explanation.md → bug explanation and fix reasoning
```

---

## Notes

- Dependency versions are pinned exactly in `package.json`.
- Lockfiles are intentionally excluded per assignment requirements.
- Tests demonstrate the bug and verify the minimal fix.
