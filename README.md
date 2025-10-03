# React Router — Beginner-Friendly README

A step‑by‑step, beginner‑friendly guide to learn **React Router (v6.4+)** from zero to practical apps.

> If you can build everything in the **“Milestone Project”** near the end, you’ve covered the core concepts.

---

## Table of Contents
1. [What is React Router?](#what-is-react-router)
2. [Install & Setup](#install--setup)
3. [Two Ways to Use React Router](#two-ways-to-use-react-router)
   - [Declarative Router (BrowserRouter)](#declarative-router-browserrouter)
   - [Data Router (createBrowserRouter)](#data-router-createbrowserrouter)
4. [Core Building Blocks](#core-building-blocks)
   - [`<Routes>` and `<Route>`](#routes-and-route)
   - [`<Link>` & `<NavLink>`](#link--navlink)
   - [`<Outlet>` & Nested Routes](#outlet--nested-routes)
   - [`useNavigate`](#usenavigate)
   - [`useParams`](#useparams)
   - [`useLocation` & `useSearchParams`](#uselocation--usesearchparams)
5. [Layouts, Index Routes & 404s](#layouts-index-routes--404s)
6. [Redirects & Guarded (Protected) Routes](#redirects--guarded-protected-routes)
7. [Code Splitting: Lazy Routes](#code-splitting-lazy-routes)
8. [Data APIs (Loaders, Actions, Fetchers)](#data-apis-loaders-actions-fetchers)
9. [Forms with Actions](#forms-with-actions)
10. [HashRouter vs BrowserRouter](#hashrouter-vs-browserrouter)
11. [Project Structure Suggestions](#project-structure-suggestions)
12. [Milestone Project: Mini Blog](#milestone-project-mini-blog)
13. [Common Pitfalls & Debugging](#common-pitfalls--debugging)
14. [Cheat Sheet](#cheat-sheet)
15. [Glossary](#glossary)

---

## What is React Router?
**React Router** is the standard routing library for React apps. It lets you:
- Map **URLs** to **UI components** (pages/screens)
- Navigate without full page reloads
- Read URL **params** and **query strings**
- Fetch/submit data tied to routes (with **Data Router** APIs)

---

## Install & Setup
```bash
# with npm
npm i react-router-dom

# or with pnpm
dpnpm add react-router-dom

# or with yarn
yarn add react-router-dom
```

Make sure you have a React app (e.g., `npm create vite@latest` or `create-react-app`).

---

## Two Ways to Use React Router
React Router offers two primary approaches:

### Declarative Router (BrowserRouter)
Quick to start; great for learning and smaller apps.

```tsx
// main.tsx (or index.tsx)
import React from 'react'
import ReactDOM from 'react-dom/client'
import { BrowserRouter } from 'react-router-dom'
import App from './App'

ReactDOM.createRoot(document.getElementById('root')!).render(
  <React.StrictMode>
    <BrowserRouter>
      <App />
    </BrowserRouter>
  </React.StrictMode>
)
```

```tsx
// App.tsx
import { Routes, Route } from 'react-router-dom'
import Home from './pages/Home'
import About from './pages/About'

export default function App() {
  return (
    <Routes>
      <Route path="/" element={<Home />} />
      <Route path="/about" element={<About />} />
    </Routes>
  )
}
```

### Data Router (createBrowserRouter)
Modern, full‑featured routing with **loaders**, **actions**, **deferred** data, and **error boundaries**.

```tsx
// main.tsx
import React from 'react'
import ReactDOM from 'react-dom/client'
import {
  createBrowserRouter,
  RouterProvider,
} from 'react-router-dom'
import RootLayout, { loader as rootLoader } from './routes/RootLayout'
import ErrorPage from './routes/ErrorPage'
import Home from './routes/Home'

const router = createBrowserRouter([
  {
    path: '/',
    element: <RootLayout />,
    loader: rootLoader,
    errorElement: <ErrorPage />,
    children: [
      { index: true, element: <Home /> },
      // more children here
    ],
  },
])

ReactDOM.createRoot(document.getElementById('root')!).render(
  <React.StrictMode>
    <RouterProvider router={router} />
  </React.StrictMode>
)
```

---

## Core Building Blocks

### `<Routes>` and `<Route>`
- `<Routes>` picks the **best matching** `<Route>`.
- `<Route path="/about" element={<About/>} />` maps a URL to a component.
- **Index routes** have no `path` and render at the parent’s path.

```tsx
<Routes>
  <Route path="/" element={<Layout />}>
    <Route index element={<Home />} />
    <Route path="about" element={<About />} />
    <Route path="contact" element={<Contact />} />
    <Route path="*" element={<NotFound />} />
  </Route>
</Routes>
```

### `<Link>` & `<NavLink>`
- `<Link to="/about">About</Link>` navigates without reload.
- `<NavLink>` is like Link but adds **active** styling.

```tsx
import { Link, NavLink } from 'react-router-dom'

<nav>
  <NavLink to="/" end>Home</NavLink>
  <NavLink to="/about">About</NavLink>
  <Link to="/contact">Contact</Link>
</nav>
```

### `<Outlet>` & Nested Routes
- Use `<Outlet />` where **child routes** should render.

```tsx
// Layout.tsx
import { Outlet } from 'react-router-dom'

export default function Layout() {
  return (
    <div>
      <header>My Site</header>
      <main>
        <Outlet />
      </main>
    </div>
  )
}
```

### `useNavigate`
Programmatic navigation.

```tsx
import { useNavigate } from 'react-router-dom'

function SaveButton() {
  const navigate = useNavigate()
  return (
    <button onClick={() => navigate('/dashboard')}>Save & Go</button>
  )
}
```

### `useParams`
Get route parameters like `/users/:id`.

```tsx
// route: /users/:id
import { useParams } from 'react-router-dom'

function User() {
  const { id } = useParams()
  // fetch user by id
  return <div>User ID: {id}</div>
}
```

### `useLocation` & `useSearchParams`
- `useLocation()` returns the current location (pathname, search, state).
- `useSearchParams()` reads/sets query string params.

```tsx
import { useLocation, useSearchParams } from 'react-router-dom'

function SearchPage() {
  const location = useLocation()
  const [searchParams, setSearchParams] = useSearchParams()
  const q = searchParams.get('q') || ''

  return (
    <section>
      <p>Current path: {location.pathname}</p>
      <input
        value={q}
        onChange={(e) => setSearchParams({ q: e.target.value })}
        placeholder="Search..."
      />
    </section>
  )
}
```

---

## Layouts, Index Routes & 404s
- Put shared UI (nav, footer) in a **layout route**.
- Use **index routes** for default children.
- Catch all unmatched paths with `path="*"`.

```tsx
<Routes>
  <Route path="/" element={<Layout />}>
    <Route index element={<Home />} />
    <Route path="products">
      <Route index element={<ProductsList />} />
      <Route path=":productId" element={<ProductDetail />} />
    </Route>
    <Route path="*" element={<NotFound />} />
  </Route>
</Routes>
```

---

## Redirects & Guarded (Protected) Routes

### Redirects
```tsx
import { Navigate } from 'react-router-dom'

function LegacyPage() {
  return <Navigate to="/new-home" replace />
}
```

### Protected Routes (simple pattern)
```tsx
// RequireAuth.tsx
import { Navigate, Outlet, useLocation } from 'react-router-dom'

export default function RequireAuth({ isAuthed }: { isAuthed: boolean }) {
  const location = useLocation()
  if (!isAuthed) {
    return <Navigate to="/login" state={{ from: location }} replace />
  }
  return <Outlet />
}
```

```tsx
// usage
<Routes>
  <Route element={<RequireAuth isAuthed={isLoggedIn} />}>
    <Route path="/dashboard" element={<Dashboard />} />
  </Route>
  <Route path="/login" element={<Login />} />
</Routes>
```

---

## Code Splitting: Lazy Routes
Load screens on demand for faster initial loads.

```tsx
import { Suspense, lazy } from 'react'
import { Routes, Route } from 'react-router-dom'

const Home = lazy(() => import('./pages/Home'))
const About = lazy(() => import('./pages/About'))

export default function App() {
  return (
    <Suspense fallback={<div>Loading…</div>}>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
      </Routes>
    </Suspense>
  )
}
```

> With **Data Router**, you can also lazy‑load route modules via `lazy:` in the route config.

---

## Data APIs (Loaders, Actions, Fetchers)
These are available in the **Data Router** (`createBrowserRouter`). They attach data fetching and mutations **to routes**, not components.

### Loader (read data before rendering)
```tsx
// routes/Users.tsx
import { useLoaderData } from 'react-router-dom'

export async function loader() {
  const res = await fetch('/api/users')
  return res.json()
}

export default function Users() {
  const users = useLoaderData() as { id: number; name: string }[]
  return (
    <ul>
      {users.map(u => <li key={u.id}>{u.name}</li>)}
    </ul>
  )
}
```

### Action (write data on navigation or form submit)
```tsx
// routes/NewUser.tsx
import { Form, redirect, useActionData } from 'react-router-dom'

export async function action({ request }: { request: Request }) {
  const formData = await request.formData()
  const res = await fetch('/api/users', {
    method: 'POST',
    body: formData,
  })
  if (!res.ok) return { error: 'Failed to create user' }
  return redirect('/users')
}

export default function NewUser() {
  const data = useActionData() as { error?: string } | undefined
  return (
    <div>
      <h2>New User</h2>
      {data?.error && <p style={{ color: 'red' }}>{data.error}</p>}
      <Form method="post">
        <input name="name" placeholder="Name" />
        <button type="submit">Create</button>
      </Form>
    </div>
  )
}
```

### Error Boundaries
Render friendly errors per route.

```tsx
// routes/ErrorPage.tsx
import { isRouteErrorResponse, useRouteError } from 'react-router-dom'

export default function ErrorPage() {
  const error = useRouteError()
  if (isRouteErrorResponse(error)) {
    return (
      <div>
        <h1>{error.status} {error.statusText}</h1>
        <p>{error.data?.message || 'Something went wrong'}</p>
      </div>
    )
  }
  return <div>Unexpected error</div>
}
```

### Defer & Streaming (advanced, optional)
```tsx
import { defer, Await, useLoaderData } from 'react-router-dom'

export function loader() {
  const usersPromise = fetch('/api/users').then(r => r.json())
  return defer({ users: usersPromise })
}

export default function Users() {
  const data = useLoaderData() as { users: Promise<any[]> }
  return (
    <React.Suspense fallback={<p>Loading users…</p>}>
      <Await resolve={data.users}>
        {(users) => users.map(u => <div key={u.id}>{u.name}</div>)}
      </Await>
    </React.Suspense>
  )
}
```

---

## Forms with Actions
`<Form>` from React Router automatically posts to the current route’s **action**.

```tsx
import { Form } from 'react-router-dom'

export default function ContactForm() {
  return (
    <Form method="post">
      <input name="email" type="email" required />
      <textarea name="message" required />
      <button type="submit">Send</button>
    </Form>
  )
}
```

---

## HashRouter vs BrowserRouter
- **BrowserRouter** (recommended): clean URLs (e.g., `/about`). Needs server configured to serve `index.html` for all routes.
- **HashRouter**: uses `#` fragment (e.g., `/#/about`). Works on static hosts without special server config.

```tsx
import { HashRouter } from 'react-router-dom'

<HashRouter>
  <App />
</HashRouter>
```

---

## Project Structure Suggestions
```
src/
  routes/
    RootLayout.tsx
    ErrorPage.tsx
    Home.tsx
    Users.tsx
    NewUser.tsx
  pages/           # if using BrowserRouter style
  components/
  lib/
  main.tsx
  App.tsx
```

---

## Milestone Project: Mini Blog
Build a tiny blog to practice everything.

### Requirements
1. Routes:
   - `/` → Home (lists posts, from loader)
   - `/posts/:id` → Post detail (loader uses `id`)
   - `/new` → New post (Form + action → redirect to `/`)
   - `*` → 404
2. Layout with nav + `<Outlet/>`.
3. Active link styling via `<NavLink/>`.
4. Programmatic nav after creating a post.
5. Use `useSearchParams` for searching posts.

### Starter Snippets
```tsx
// routes/RootLayout.tsx
import { NavLink, Outlet } from 'react-router-dom'
export default function RootLayout() {
  return (
    <div>
      <nav>
        <NavLink to="/" end>Home</NavLink>
        <NavLink to="/new">New</NavLink>
      </nav>
      <Outlet />
    </div>
  )
}
```

```tsx
// routes/Home.tsx
import { useLoaderData, Link, json } from 'react-router-dom'

export async function loader({ request }: { request: Request }) {
  const url = new URL(request.url)
  const q = url.searchParams.get('q') || ''
  const res = await fetch(`/api/posts?q=${encodeURIComponent(q)}`)
  if (!res.ok) throw json({ message: 'Failed posts' }, { status: 500 })
  return res.json()
}

export default function Home() {
  const posts = useLoaderData() as { id: number; title: string }[]
  return (
    <ul>
      {posts.map(p => (
        <li key={p.id}>
          <Link to={`/posts/${p.id}`}>{p.title}</Link>
        </li>
      ))}
    </ul>
  )
}
```

```tsx
// routes/PostDetail.tsx
import { useLoaderData, json } from 'react-router-dom'

export async function loader({ params }: { params: any }) {
  const res = await fetch(`/api/posts/${params.id}`)
  if (!res.ok) throw json({ message: 'Post not found' }, { status: 404 })
  return res.json()
}

export default function PostDetail() {
  const post = useLoaderData() as { id: number; title: string; body: string }
  return (
    <article>
      <h1>{post.title}</h1>
      <p>{post.body}</p>
    </article>
  )
}
```

```tsx
// routes/NewPost.tsx
import { Form, redirect, useActionData } from 'react-router-dom'

export async function action({ request }: { request: Request }) {
  const formData = await request.formData()
  const res = await fetch('/api/posts', { method: 'POST', body: formData })
  if (!res.ok) return { error: 'Could not create post' }
  return redirect('/')
}

export default function NewPost() {
  const data = useActionData() as { error?: string } | undefined
  return (
    <div>
      <h2>New Post</h2>
      {data?.error && <p style={{ color: 'red' }}>{data.error}</p>}
      <Form method="post">
        <input name="title" placeholder="Title" required />
        <textarea name="body" placeholder="Body" required />
        <button type="submit">Create</button>
      </Form>
    </div>
  )
}
```

### Wire-Up the Router
```tsx
// main.tsx (Data Router style)
import React from 'react'
import ReactDOM from 'react-dom/client'
import { createBrowserRouter, RouterProvider } from 'react-router-dom'
import RootLayout from './routes/RootLayout'
import ErrorPage from './routes/ErrorPage'
import Home, { loader as homeLoader } from './routes/Home'
import PostDetail, { loader as postLoader } from './routes/PostDetail'
import NewPost, { action as newPostAction } from './routes/NewPost'

const router = createBrowserRouter([
  {
    path: '/',
    element: <RootLayout />,
    errorElement: <ErrorPage />,
    children: [
      { index: true, element: <Home />, loader: homeLoader },
      { path: 'posts/:id', element: <PostDetail />, loader: postLoader },
      { path: 'new', element: <NewPost />, action: newPostAction },
      { path: '*', element: <div>Not Found</div> },
    ],
  },
])

ReactDOM.createRoot(document.getElementById('root')!).render(
  <React.StrictMode>
    <RouterProvider router={router} />
  </React.StrictMode>
)
```

---

## Common Pitfalls & Debugging
- **Nothing renders?** Ensure you wrapped your app with a router (`BrowserRouter` or `RouterProvider`).
- **404s on refresh (BrowserRouter)?** Configure your server to serve `index.html` for unknown routes.
- **Wrong nested paths?** Child routes are **relative**: `path:"settings"` (not `/settings`) when nested.
- **`useParams` undefined?** Check your `path` includes the `:param` and the route is nested correctly.
- **Loader not running?** Only runs in **Data Router**. Ensure `createBrowserRouter` + route object has a `loader`.

---

## Cheat Sheet
- **Match**: `<Routes><Route path="/a" element={<A/>}/></Routes>`
- **Link**: `<Link to="/a">Go</Link>`
- **NavLink active**: `<NavLink to="/a" end className={({isActive})=> isActive? 'active':''}/>`
- **Params**: `const { id } = useParams()`
- **Navigate**: `useNavigate()('/login')`
- **Query**: `const [sp, setSp] = useSearchParams()`
- **Nested**: Parent renders `<Outlet/>`, children define relative `path`
- **Index route**: `{ index: true, element: <Home/> }`
- **Fallback (404)**: `path:"*"`
- **Redirect**: `<Navigate to="/" replace />`
- **Loader**: `loader: async ({request, params}) => {}`
- **Action**: `action: async ({request, params}) => {}`
- **Error UI**: `errorElement: <ErrorPage/>`

---

## Glossary
- **Route**: A rule connecting a URL path to a UI component.
- **Layout Route**: A route that renders shared UI and an `<Outlet/>` for children.
- **Index Route**: Default child shown at a parent’s exact path.
- **Loader**: Route‑level data fetch.
- **Action**: Route‑level data mutation handler.
- **Outlet**: Where child routes render.
- **Navigate**: Programmatic route change.
- **Search Params**: Query string key/value pairs in the URL.

---

Happy routing! 🚀

