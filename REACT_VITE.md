# Vite + React (JavaScript) — Quickstart

This README shows how to scaffold, run, build, and preview a **React + Vite** project using JavaScript.

---

## Prerequisites
- **Node.js** ≥ 18 (LTS recommended)
- **npm** (comes with Node). Or use **pnpm**/**yarn** if you prefer (examples below).

Check versions:
```bash
node -v
npm -v
```

---

## 1) Create a New Project
Use Vite’s project creator, select **React**, then **JavaScript**.

```bash
npm create vite@latest
# ? Project name:  my-project
# ? Select a framework:  React
# ? Select a variant:    JavaScript
```

Then move into the new folder:
```bash
cd my-project
```

Install dependencies:
```bash
npm install
```

---

## 2) Start the Dev Server
Runs a fast dev server with hot reload (HMR).

```bash
npm run dev
```

By default Vite prints a local URL like:
```
  ➜  Local:   http://localhost:5173/
```
Open that in your browser.

> **Tip:** Press `q` in the terminal to quit the dev server, or `Ctrl+C` to stop.

---

## 3) Build for Production
Outputs an optimized static build into `dist/`.

```bash
npm run build
```

Preview the production build locally:
```bash
npm run preview
# Opens a local server to serve the dist/ folder
```

---

## 4) Useful Scripts (package.json)
These are created by Vite:

```json
{
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview"
  }
}
```

---

## 5) Project Structure (after create)
```
my-project/
├─ index.html
├─ package.json
├─ vite.config.js           # Vite configuration
├─ /src
│  ├─ main.jsx              # App entry
│  ├─ App.jsx               # Root component
│  └─ assets/               # Static assets
└─ /public                  # (optional) served at /, copied as-is
```

---

## 6) Change the Dev Port (optional)
Default port is **5173**. To change it, edit `vite.config.js`:

```js
// vite.config.js
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

export default defineConfig({
  plugins: [react()],
  server: {
    port: 3000
  }
})
```

Run again:
```bash
npm run dev
# Now: http://localhost:3000
```

---

## 7) Using pnpm or yarn (optional)
If you prefer **pnpm**:
```bash
pnpm create vite
cd my-project
pnpm install
pnpm dev
```

If you prefer **yarn**:
```bash
yarn create vite
cd my-project
yarn
yarn dev
```

---

## 8) Environment Variables (optional)
Create a `.env` file in the project root:
```env
VITE_API_URL=https://api.example.com
```
Access it in code (variables must start with `VITE_`):
```jsx
console.log(import.meta.env.VITE_API_URL)
```

---

## 9) Add React Router (optional)
Install and create basic routes:
```bash
npm i react-router-dom
```
```jsx
// src/main.jsx
import { createRoot } from 'react-dom/client'
import { createBrowserRouter, RouterProvider } from 'react-router-dom'
import App from './App.jsx'

const router = createBrowserRouter([
  { path: '/', element: <App /> },
])

createRoot(document.getElementById('root')).render(
  <RouterProvider router={router} />
)
```

---

## 10) Troubleshooting
- **Port already in use**: change the port in `vite.config.js` (see above) or stop the other process.
- **Old Node/npm**: upgrade Node to the latest LTS.
- **Blank page in production**: ensure your deploy host serves the built `dist/` folder and respects relative paths. For GitHub Pages, consider setting `base` in `vite.config.js`.

---

## 11) Deploy (quick notes)
- **Static hosts** (Netlify, Vercel, GitHub Pages): build with `npm run build` and deploy `dist/`.
- **Custom server**: serve `dist/` as static files via Nginx/Express/etc.

---

## License
MIT (or your preferred license)

---

Happy hacking! 🚀

