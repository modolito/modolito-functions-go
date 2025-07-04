
# Modolito Go Function Template

This is the official **Modolito function template** for the Go runtime.

It provides a minimal structure for building Go-based functions that comply with the Modolito platform, using only the Go 1.22+ standard library.

---

## Project Structure

```
.
├── go.mod             # Go module definition
├── main.go            # Function entrypoint
├── modolito.json      # Function manifest (used by Modolito CLI and platform)
```

## How it works

This function exposes a single HTTP endpoint:

```
POST /hello
```

It accepts a JSON payload like:

```json
{
  "name": "Alice"
}
```

And returns, in this case:

```json
{
  "message": "Hello, Alice!"
}
```

## modolito.json

The `modolito.json` manifest describes the runtime, entrypoint, and available function(s). It is used by Modolito to build, deploy, and document your function.

```json
{
  "runtime": "go",
  "entrypoint": "main.go",
  "functions": [
    {
      "name": "hello",
      "description": "A simple function that returns a greeting message.",
      "parameters": [
        {
          "name": "name",
          "type": "string"
        }
      ],
      "returns": [
        {
          "name": "message",
          "type": "string"
        }
      ]
    }
  ]
}
```

## Run locally

```
go run main.go
```

Then test it using `curl` or any HTTP client:

```
curl -X POST http://localhost:8080/hello \
  -H "Content-Type: application/json" \
  -d '{"name": "Alice"}'
```

## Modolito Function Requirements

For your application to be considered a valid Modolito Function, it must follow these conventions:

- A required file named **`modolito.json`** must declare the `runtime`, `entrypoint`, and a list of `functions`.
- Each function **must expose a POST HTTP endpoint** at the path `/function-name`.
- All input `parameters` are passed as a **JSON object in the request body**.
- All `returns` must be encoded as a **JSON object in the response body**.
- All responses must follow these standard HTTP status codes:
  - `2xx` for successful operations
  - `4xx` for business logic errors (e.g., validation errors)
  - `5xx` for internal errors (e.g., database connection issues)

### Error Response Format

Modolito expects error responses to follow a specific JSON structure:

**Example of a 4xx error response:**

```json
{
  "error": {
    "code": "invalid_input",
    "message": "Email already exists."
  }
}
```

**Example of a 5xx error response:**

```json
{
  "error": {
    "code": "internal_error",
    "message": "Database connection failed."
  }
}
```

## License

This project is licensed under the [MIT License](LICENSE).
