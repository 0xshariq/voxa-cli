# Voxa CLI (Future)

> **Note:** This is a planned feature currently under development.

## Command Structure

### 1. Instance-Based Commands

These commands use a Voxa instance for advanced features and persistent configuration.

- **Get:**

  ```sh
  voxa get <url> <payload|null> <headers>
  ```

  - `payload` should be `null` for GET, HEAD, OPTIONS, and DELETE requests.
  - Example:
    ```sh
    voxa get https://api.example.com/users null '{"Authorization": "Bearer token"}'
    ```

- **Put:**
  ```sh
  voxa put <url> <payload> <headers>
  ```
- **Post:**
  ```sh
  voxa post <url> <payload> <headers>
  ```
- **Patch:**
  ```sh
  voxa patch <url> <payload> <headers>
  ```
- **Delete:**
  ```sh
  voxa delete <url> null <headers>
  ```
- **Options:**
  ```sh
  voxa options <url> null <headers>
  ```
- **Head:**
  ```sh
  voxa head <url> null <headers>
  ```

### 2. Static Request Command

This command uses Voxa static methods for stateless, one-off requests.

- **Request:**
  ```sh
  voxa request <method> <url> <payload> <headers>
  ```
  - `method` can be GET, POST, PUT, PATCH, DELETE, OPTIONS, HEAD
  - Example:
    ```sh
    voxa request POST https://api.example.com/users '{"name": "John"}' '{"Authorization": "Bearer token"}'
    voxa request GET https://api.example.com/users null '{"Authorization": "Bearer token"}'
    ```

## Dual-Mode CLI: Developer & Cloud

### Project Structure

- `index.ts`: Entry point for the CLI. Parses command-line arguments, initializes configuration, and dispatches commands.
- `commands/`: Contains individual command modules (e.g., `get.ts`, `post.ts`, `request.ts`). Each module implements the logic for its respective command.
- `docs/`: Documentation for CLI usage and features.
- `config/`: (Optional) Stores global or environment-specific configuration files.

### How Dual-Mode Works

1. **Initialization**

   - The CLI is started via `index.ts`, which reads arguments, environment variables, and flags (e.g., `--api <url>`).
   - Configuration (e.g., baseURL, cache, headers) is loaded and validated.

2. **Mode Selection**

   - If the `--api <url>` flag or config is provided, the CLI routes commands to the remote Voxa API (cloud mode).
   - If not, the CLI uses the local Voxa client library for direct execution (developer mode).

3. **Command Dispatch**

   - Based on the first argument (e.g., `get`, `post`, `request`), the CLI loads the corresponding module from `commands/`.
   - Each command module exports a function that receives parsed arguments (URL, payload, headers, etc.).

4. **Instance vs. Static Logic**

   - Instance-based commands (`get`, `put`, `post`, `patch`, `delete`, `options`, `head`) use a persistent Voxa instance, allowing advanced features (caching, retry, queue, etc.).
   - The `request` command uses Voxa static methods for stateless, one-off requests with optional config.

5. **Execution & Output**

   - In developer mode, commands use the local Voxa client for HTTP requests and print output directly.
   - In cloud mode, commands send HTTP requests to the remote API, which executes the logic and returns results.
   - Errors are caught and displayed in a consistent format.

6. **Extensibility**
   - New commands can be added by creating modules in `commands/` and updating `index.ts` to recognize them.
   - Advanced features (batch, stats, etc.) can be implemented as additional commands.

### Example Workflow

```sh
# Developer mode (local Voxa client)
voxa get https://api.example.com/users null '{"Authorization": "Bearer token"}'

# Cloud mode (remote Voxa API)
voxa get https://api.example.com/users null '{"Authorization": "Bearer token"}' --api https://your-voxa-api

# Stateless POST (cloud mode)
voxa request POST https://api.example.com/users '{"name": "John"}' '{"Authorization": "Bearer token"}' --api https://your-voxa-api
```

---

For implementation details, see `index.ts` and the `commands/` folder. 4. **Incremental:** Build features step-by-step

### Key Differentiators

To stand out from Postman:

1. **Open Source:** Make it fully open source
2. **Performance:** Focus on speed (Redis caching, queue management)
3. **Developer-First:** CLI-first, then GUI
4. **TypeScript Native:** Full TypeScript support, type generation
5. **Modern Stack:** Electron + React + Voxa
6. **Privacy:** No forced cloud sync, local-first
7. **Extensibility:** Plugin system for custom features

### Competitive Landscape

- **Postman:** Market leader, feature-rich, cloud-based
- **Insomnia:** Lighter alternative, Kong-owned
- **Hoppscotch:** Open-source, web-based
- **HTTPie Desktop:** Modern, beautiful UI
- **Thunder Client:** VS Code extension

**Your Opportunity:** CLI-first approach + TypeScript-native + open-source

## Next Steps

See [CONFIGURATION.md](./CONFIGURATION.md) for environment setup and credential management.
