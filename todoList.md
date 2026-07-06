# Blog App — To Do

## Phase 0 — Git & GitHub Setup
- [x] Install the GitHub CLI if you don't have it: `winget install GitHub.cli`
- [x] Authenticate: `gh auth login` (opens a browser once to connect your account)
- [x] Create a new repo and clone it in one step: `gh repo create blog-app --public --clone`
- [x] `cd blog-app`
- [x] Add `CLAUDE.md`, `todoList.md`, and a root `.gitignore` to the cloned folder
- [x] `git add .` → `git commit -m "chore: add project docs and gitignore"` → `git push`

---

## Phase 1 — Project Setup
**Branch:** `git checkout -b phase/1-setup`

- [ ] Create `server/` and `client/` folders inside the repo
- [ ] Init the React app with Vite inside `client/`: `npm create vite@latest . -- --template react`
- [ ] Create `server/package.json` with `"type": "module"` and install dependencies:
  `express prisma @prisma/client @prisma/adapter-pg pg bcrypt jsonwebtoken cors cookie-parser dotenv`
- [ ] Install dev dependency: `nodemon`
- [ ] Add a `dev` script to `server/package.json`: `"dev": "nodemon src/server.js"`
- [ ] Create `server/.env` (make sure `.env` is in `.gitignore` — never commit this)
- [ ] `git add .` → `git commit -m "chore: scaffold server and client, install dependencies"`
- [ ] `git push origin phase/1-setup`
- [ ] `gh pr create --title "Phase 1: project setup" --body ""`
- [ ] `gh pr merge --squash` → `git checkout main` → `git pull`

---

## Phase 2 — Database
**Branch:** `git checkout -b phase/2-database`

- [ ] Create a PostgreSQL database locally
- [ ] Run `npx prisma init` inside `server/` — this creates `prisma/schema.prisma` and `prisma.config.ts`
- [ ] Edit `prisma.config.ts` to load `DATABASE_URL` from dotenv (do NOT put `url` in `schema.prisma`)
- [ ] Define `User` and `Post` models in `prisma/schema.prisma`:
  - `User` — id, username, passwordHash, createdAt, posts[]
  - `Post` — id, title, body, authorId (FK → User), createdAt, author
- [ ] Set `output` in schema to `../generated/prisma` so the client generates outside `node_modules`
- [ ] Run `npx prisma migrate dev --name init` to create the tables
- [ ] Run `npx prisma generate` to generate the Prisma client
- [ ] Create `server/src/db/index.js` — `pg.Pool` + `PrismaPg` adapter + `PrismaClient` export
- [ ] `git add .` → `git commit -m "feat: Prisma schema, migration, and db client"`
- [ ] `git push origin phase/2-database`
- [ ] `gh pr create --title "Phase 2: database setup" --body ""`
- [ ] `gh pr merge --squash` → `git checkout main` → `git pull`

---

## Phase 3 — Server Structure
**Branch:** `git checkout -b phase/3-server`

- [ ] Create `server/src/server.js` — loads `.env`, starts Express on `PORT`
- [ ] Create `server/src/app.js` — sets up CORS (with `credentials: true` and `CLIENT_URL` origin), JSON parsing, cookie-parser, and mounts routes
- [ ] Smoke test: start the server with `npm run dev`, confirm it starts without errors
- [ ] `git add .` → `git commit -m "feat: Express entry point and middleware setup"`
- [ ] `git push origin phase/3-server`
- [ ] `gh pr create --title "Phase 3: server structure" --body ""`
- [ ] `gh pr merge --squash` → `git checkout main` → `git pull`

---

## Phase 4 — Auth (Backend)
**Branch:** `git checkout -b phase/4-auth-backend`

- [ ] `server/src/middleware/auth.middleware.js` — reads `token` cookie, verifies JWT, attaches `{ userId, username }` to `req.user`
- [ ] `server/src/controllers/auth.controller.js`:
  - `register` — hash password with bcrypt, create user in DB
  - `login` — verify password, sign JWT, set httpOnly cookie named `token`, respond with `{ userId, username }`
  - `logout` — clear the cookie
  - `getMe` — return `req.user` (protected by middleware)
- [ ] `server/src/routes/auth.routes.js` — wire up POST `/register`, POST `/login`, POST `/logout`, GET `/me`
- [ ] Mount auth routes in `app.js` under `/auth`
- [ ] Test all four endpoints with Thunder Client before committing
- [ ] `git add .` → `git commit -m "feat: auth register, login, logout, and getMe endpoints"`
- [ ] `git push origin phase/4-auth-backend`
- [ ] `gh pr create --title "Phase 4: auth backend" --body ""`
- [ ] `gh pr merge --squash` → `git checkout main` → `git pull`

---

## Phase 5 — Blog Posts (Backend)
**Branch:** `git checkout -b phase/5-posts-backend`

- [ ] `server/src/controllers/posts.controller.js`:
  - `getAllPosts` — return all posts (include `author.username`)
  - `getPostById` — return single post by id
  - `createPost` — create post with `authorId` from `req.user.userId`
  - `deletePost` — delete only if `req.user.userId` matches `post.authorId`
- [ ] `server/src/routes/posts.routes.js` — GET `/` and GET `/:id` are public; POST `/` and DELETE `/:id` apply `authenticateToken`
- [ ] Mount posts routes in `app.js` under `/posts`
- [ ] Test all four endpoints with Thunder Client before committing
- [ ] `git add .` → `git commit -m "feat: posts CRUD endpoints"`
- [ ] `git push origin phase/5-posts-backend`
- [ ] `gh pr create --title "Phase 5: posts backend" --body ""`
- [ ] `gh pr merge --squash` → `git checkout main` → `git pull`

---

## Phase 6 — React Frontend
**Branch:** `git checkout -b phase/6-frontend`

- [ ] Install client dependency: `react-router-dom`
- [ ] `client/src/main.jsx` — wrap `<App />` in `<BrowserRouter>`
- [ ] `client/src/context/AuthContext.jsx` — provides `user`, `setUser`, `isLoading`; fetches `GET /auth/me` on mount to restore session
- [ ] `client/src/App.jsx` — wraps everything in `<AuthProvider>`, renders `<Navbar>` and all `<Route>` entries
- [ ] `git add .` → `git commit -m "feat: app shell, router, and auth context"`
- [ ] `client/src/components/Navbar.jsx` — Login/Register links when logged out; Welcome + Logout button when logged in
- [ ] `git add .` → `git commit -m "feat: Navbar component"`
- [ ] `client/src/pages/Register.jsx` — form → POST `/auth/register`, redirect to `/login` on success
- [ ] `client/src/pages/Login.jsx` — form → POST `/auth/login`, call `setUser` on success, redirect to `/`
- [ ] `git add .` → `git commit -m "feat: Register and Login pages"`
- [ ] `client/src/pages/Home.jsx` — fetch GET `/posts`, list posts with links, "New Post" button visible only when logged in
- [ ] `client/src/pages/NewPost.jsx` — form → POST `/posts`, redirect to `/` on success
- [ ] `client/src/pages/PostDetail.jsx` — fetch GET `/posts/:id`, show post, delete button visible only if `user.userId === post.authorId`
- [ ] `git add .` → `git commit -m "feat: Home, NewPost, and PostDetail pages"`
- [ ] `git push origin phase/6-frontend`
- [ ] `gh pr create --title "Phase 6: React frontend" --body ""`
- [ ] `gh pr merge --squash` → `git checkout main` → `git pull`

---

## Phase 7 — Polish
**Branch:** `git checkout -b phase/7-polish`

- [ ] `client/src/components/ProtectedRoute.jsx` — if not logged in, redirect to `/login`; wrap the `/newPost` route in `App.jsx`
- [ ] Add "New Post" link to Navbar (only visible when logged in)
- [ ] Logout button in Navbar — POST `/auth/logout` with `credentials: 'include'`, then call `setUser(null)` and redirect to `/`
- [ ] End-to-end test: register → login → create post → view post → delete post → logout
- [ ] `git add .` → `git commit -m "feat: protected route, new post link, and logout"`
- [ ] `git push origin phase/7-polish`
- [ ] `gh pr create --title "Phase 7: polish" --body ""`
- [ ] `gh pr merge --squash` → `git checkout main` → `git pull`
