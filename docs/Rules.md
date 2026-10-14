# CampusSphere - Engineering Rules & AI Boundaries

## 1. Core Engineering Principles

1. **Keep Client and Server Strictly Separate**: No server-side logic (DB queries, secrets) in `client/`. No client-side assumptions (DOM, window) in `server/`.
2. **Explicit Over Implicit**: Use named exports, explicit return types (JSDoc or TypeScript where adopted), and clear variable names. Avoid "clever" one-liners that obscure intent.
3. **Colocation**: Keep component-specific helpers, sub-components, and local styles close to where they are consumed.
4. **Defensive Programming**: Validate every external boundary — user input (Zod), URL params, API responses, and file uploads — before processing.
5. **Preserve Documentation & Comments**: Always keep existing comments, docstrings, and architectural rationale unless explicitly instructed to change them.

---

## 2. What to Use (Best Practices)

### Frontend (`client/`)
- **Routing**: `react-router-dom` — use `<Outlet>` layouts for protected/admin wrappers.
- **Data Fetching**: `@tanstack/react-query` for all server state (caching, deduplication, background refetch, optimistic updates).
- **HTTP Client**: A single **Axios instance** (`src/api/axios.js`) with `baseURL = import.meta.env.VITE_API_URL` and a request interceptor attaching the Bearer token.
- **Auth State**: `AuthContext` + `useAuth` hook — provides `user`, `login(token)`, `logout()`. Token stored in `localStorage`.
- **Forms**: Controlled inputs with local `useState` or a lightweight form library. Show field-level validation errors from the API response next to the relevant input.
- **Real-time**: `socket.io-client` via a dedicated `useSocket` hook — connect once on auth, auto-reconnect.
- **Styling**: Tailwind CSS utilities only. Use `clsx` for dynamic conditional classes. Follow colour tokens from `Design.md`.
- **Icons**: `lucide-react` exclusively.
- **Notifications (UI)**: `react-hot-toast` or `sonner` for success/error feedback on mutations.

### Backend (`server/`)
- **Validation**: Zod schemas for every `req.body` in routes — reject with 400 + field errors before reaching the controller.
- **Password Hashing**: `bcrypt` with salt rounds ≥ 10.
- **JWT**: Sign with `JWT_SECRET` from env; include `{id, role}` in payload; set `expiresIn: '7d'`.
- **Database Access**: All queries through **Prisma Client** singleton (`src/lib/prisma.js`). Use `$transaction(...)` for any multi-step write (booking, review + rating recalculation).
- **Error Responses**: Always use the central error handler. Operational errors (4xx) vs programmer errors (5xx) must be distinguished.
- **Logging**: Use `console.error` for 5xx; keep 4xx silent or at `warn` level.

### Shared
- **Environment Variables**: All config via `process.env` (server) or `import.meta.env` (client). Never hardcode URLs, credentials, or API keys.
- **`.env.example`**: Every variable must be documented here with a descriptive placeholder (not a real value).

---

## 3. What to Avoid (Anti-Patterns)

- ❌ **No raw SQL strings** — use Prisma query methods only.
- ❌ **No `CLOUDINARY_API_SECRET` on the client** — browser must call `/api/upload/sign` to get a time-limited signature.
- ❌ **No secrets in `localStorage`** — only the JWT token is acceptable; never store DB URLs or API keys client-side.
- ❌ **No unprotected admin routes** — always chain `verifyToken → requireRole('ADMIN')`.
- ❌ **No unhandled Promise rejections** — always `try/catch` in async route handlers or use an async wrapper middleware.
- ❌ **No stack traces in production responses** — the central error handler must omit `err.stack` when `NODE_ENV === 'production'`.
- ❌ **No Moment.js** — use `date-fns` for date formatting.
- ❌ **No direct `npm install` of unapproved libraries** — stick to the approved stack in `Architecture.md` unless explicitly discussed.
- ❌ **No destructive Prisma commands without consent** — never run `prisma migrate reset` or `prisma db push --force-reset` in production.
- ❌ **No global state for server data** — use React Query; reserve Zustand/Context for pure UI state (sidebar open, active modal, etc.).

---

## 4. Error Handling Standards

### API Response Envelope
All API responses must follow this shape:

```js
// Success
{ success: true, data: <T>, message?: string }

// Failure
{ success: false, error: { code: string, message: string, details?: any } }
```

### Common Error Codes
| Code | HTTP Status | Meaning |
| :--- | :--- | :--- |
| `VALIDATION_ERROR` | 400 | Zod schema failed |
| `UNAUTHORIZED` | 401 | Missing or invalid token |
| `FORBIDDEN` | 403 | Authenticated but insufficient role/ownership |
| `NOT_FOUND` | 404 | Resource does not exist |
| `CONFLICT` | 409 | Duplicate (slot already booked, review already submitted) |
| `INTERNAL_ERROR` | 500 | Unhandled server error |

### Client-Side
- Show a toast for every failed mutation.
- Show inline field errors for 400 validation responses.
- Show an empty-state + Retry button for failed list fetches (not raw JSON or a blank screen).
- Wrap major feature sections in a React Error Boundary.

---

## 5. Boundaries & Guardrails for AI Assistants

When generating, modifying, or refactoring code in this repository:

1. **No Destructive File Overwrites**: Do not delete or blindly replace entire files when only modifying specific functions. Use targeted edits.
2. **Strict File Placement**:
   - All documentation → `docs/`.
   - Frontend code → `client/src/`.
   - Backend code → `server/src/`.
   - Database schema → `server/prisma/`.
   - Root config files only (`.gitignore`, `docker-compose.yml`, `README.md`).
3. **Dependency Discipline**: Do not add packages not in `Architecture.md` without explicit approval. Do not run `npm install` in the background.
4. **No Real Credentials**: Never generate actual API keys, tokens, passwords, or connection strings. Placeholders only in `.env.example`.
5. **Database Safety**: Never run destructive Prisma commands. For schema changes always create a new named migration (`prisma migrate dev --name <desc>`).
6. **No Personal / Sensitive Information**: Do not embed machine names, local file paths, personal email addresses, or any identifying data in any committed file.
7. **Self-Verify Before Declaring Done**:
   - Confirm all import paths resolve correctly.
   - Confirm `.env.example` reflects any new env vars introduced.
   - Confirm `docs/memory.md` is updated to reflect what was completed.
