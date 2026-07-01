# CLAUDE.md — blogAppME

This is a full-stack blog app being rebuilt from scratch as a learning exercise. The user is a CS student. Always explain concepts before helping implement them, and let the user attempt things before stepping in.

## Project Structure

```
blogAppME/
  server/         — Express + Prisma backend
  client/         — React + Vite frontend
  todoList.md     — track progress here
  tools.md        — reference for all imports and tools used
```

## Stack

- **Frontend**: React + Vite, react-router-dom
- **Backend**: Node.js + Express (ES modules, `"type": "module"`)
- **Database**: PostgreSQL (local), Prisma 7 ORM
- **Auth**: JWT stored in httpOnly cookies (NOT localStorage)
- **Password hashing**: bcrypt
- **Dev server**: nodemon

## Git Workflow

- All work happens on feature branches — never commit directly to `main`
- Branch naming: `phase/1-setup`, `phase/2-database`, etc.
- Commit often with short descriptive messages as you finish each task
- When a phase is done: push the branch → open a PR on GitHub → merge into main → start next branch
- `.env` is always in `.gitignore` — never commit it

## Current Progress

- [ ] Phase 0 — Git & GitHub Setup
- [ ] Phase 1 — Project Setup
- [ ] Phase 2 — Database
- [ ] Phase 3 — Server Structure
- [ ] Phase 4 — Auth (Backend)
- [ ] Phase 5 — Blog Posts (Backend)
- [ ] Phase 6 — React Frontend
- [ ] Phase 7 — Polish

---

## Backend API Endpoints

| Method | Path | Auth required | Description |
|--------|------|---------------|-------------|
| POST | `/auth/register` | No | Register new user |
| POST | `/auth/login` | No | Login, sets httpOnly cookie |
| POST | `/auth/logout` | No | Clears the cookie |
| GET | `/auth/me` | Yes | Returns `{ userId, username }` from JWT — used by AuthContext on mount to restore session |
| GET | `/posts` | No | Get all posts |
| GET | `/posts/:id` | No | Get single post |
| POST | `/posts` | Yes | Create a post |
| DELETE | `/posts/:id` | Yes | Delete a post (owner only) |

---

## Key Implementation Details

### Prisma 7 setup
- Uses `pg` pool + `PrismaPg` adapter (not a direct connection string)
- Client generated to `server/generated/prisma/` (set `output` in schema — not `node_modules`)
- Import path in db/index.js: `../../generated/prisma/client.js`
- Do NOT put `url` in `schema.prisma` — it is handled in `prisma.config.ts` via dotenv
- `prisma` package goes in regular `dependencies`, not devDependencies

### Auth flow
- Login sets an httpOnly cookie named `token`
- `authenticateToken` middleware reads and verifies it
- Decoded payload is attached to `req.user` as `{ userId, username }`
- Protected routes apply `authenticateToken` as middleware before the controller
- **Important:** login response body AND `/auth/me` must both return `{ userId, username }` — using `id` instead of `userId` will break frontend comparisons like `user.userId === post.authorId`

### Frontend fetch pattern
All fetch calls must include `credentials: 'include'` so cookies are sent:
```js
fetch('http://localhost:3000/posts', {
  credentials: 'include',
})
```

POST requests also need:
```js
{
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  credentials: 'include',
  body: JSON.stringify({ ... }),
}
```

### CORS setup (important)
Must allow credentials and set the exact origin (not `*`):
```js
app.use(cors({
  origin: process.env.CLIENT_URL,
  credentials: true,
}))
```

---

## Client Folder Structure

```
client/
  src/
    main.jsx                      — entry point, wraps app in <BrowserRouter>
    App.jsx                       — AuthProvider, Navbar, and all Routes
    context/
      AuthContext.jsx             — provides user, setUser, isLoading. Fetches /auth/me on mount.
    components/
      Navbar.jsx                  — Login/Register when logged out, Welcome + Logout when logged in
      ProtectedRoute.jsx          — redirects to /login if not authenticated
    pages/
      Home.jsx                    — fetches GET /posts, lists posts with links
      Login.jsx                   — form → POST /auth/login, calls setUser on success
      Register.jsx                — form → POST /auth/register, redirects to /login on success
      NewPost.jsx                 — form → POST /posts, redirects to / on success
      PostDetail.jsx              — fetches GET /posts/:id, shows post, delete button if owner
```

## Server Folder Structure

```
server/
  src/
    server.js                     — entry point, loads .env, starts server on PORT
    app.js                        — Express setup, CORS, middleware, routes
    controllers/
      auth.controller.js          — register, login, logout, getMe
      posts.controller.js         — getAllPosts, getPostById, createPost, deletePost
    middleware/
      auth.middleware.js          — authenticateToken (reads cookie, attaches req.user)
    routes/
      auth.routes.js              — /auth routes
      posts.routes.js             — /posts routes
    db/
      index.js                    — pg.Pool + PrismaPg + PrismaClient export
  prisma/
    schema.prisma                 — User and Post models
  prisma.config.ts                — points Prisma at DATABASE_URL via dotenv
  generated/prisma/               — auto-generated Prisma client (never edit)
  .env                            — PORT, DATABASE_URL, JWT_SECRET, JWT_EXPIRES_IN, CLIENT_URL
```

---

## Environment Variables (server/.env)

```
PORT=3000
DATABASE_URL=postgresql://postgres:<password>@localhost:5432/<dbname>
JWT_SECRET=<secret>
JWT_EXPIRES_IN=1h
CLIENT_URL=http://localhost:5173
```

---

## Running the App

```bash
# Start the backend
cd server
npm run dev

# Start the frontend
cd client
npm run dev
```
