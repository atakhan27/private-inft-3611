## Task 1 – httpServer_crash.go Analysis

The `httpServer_crash.go` file simulates a deliberately unreliable HTTP server. Its core purpose is to randomly fail in order to model a faulty microservice under load and test error handling, SLOs, and observability.

### Key Points:
- The server starts and listens on a specified port (usually 8080).
- It handles HTTP requests to endpoints such as `/` or `/health`.
- A **random crash mechanism** is introduced in the code using Go’s `math/rand` package. The logic might be something like:
  ```go
  if rand.Float32() < 0.3 {
      panic("server crash!")
  }
