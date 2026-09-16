# nextjs-conventions-cheatsheet

> English | [繁體中文](./README_ZH-TW.md)

⬥ indicates conventional names that cannot be changed; ⬦ indicates example names that are not fixed

```mermaid
---
config:
  treeView:
    showIcons: false
    rowIndent: 16
    paddingY: 6
---
treeView-beta
⚡ nextjs-conventions/
  ⬥📁 src/ ## Auto-detects `src/app/` or `app/`; optional folder
    ⬥📁 app/
      ⬥🏛️ layout.tsx ## ＜Layout＞ [Shared Layout] Layout sidebars/footers here (root `layout.tsx` must return ＜html＞ ＜body＞)
      ⬥🔄 template.tsx ## ⠀＜Template＞ [Dynamic Layout] Like `layout.tsx`, but remounts on navigation to reset state
      ⬥🚨 error.tsx ## ⠀⠀＜ErrorBoundary＞ Shown when errors occur (must be 'use client')
      ⬥⏳ loading.tsx ## ⠀⠀⠀＜Suspense＞ Built on React Suspense; shows skeleton UI during async data fetching
      ⬥🚫 not-found.tsx ## ⠀⠀⠀⠀＜NotFound＞ Shown when route is missing (like experimental `forbidden.tsx`, `unauthorized.tsx`)
      ⬥🖥️ page.tsx ## ⠀⠀⠀⠀⠀＜Page＞ [Core UI] Maps to `/` route (defaults to Server Component)
      ⬥💥 global-error.tsx ## Shown on root `layout.tsx` error (must be 'use client' and return ＜html＞ ＜body＞)
      ⬥🌐 favicon.ico ## Includes `icon`, `apple-icon` (supports `.ico`/`.png`/`.tsx`; only `favicon.ico` belongs in `app/` root)
      ⬥🗺️ sitemap.xml ## Supports `.xml`/`.ts`; placed in `app/` root
      ⬥🤖 robots.txt ## [Crawler Rules] Supports `.txt`/`.ts`; placed in `app/` root
      ⬥📱 manifest.json ## [Web App Manifest] Supports `.json`/`.ts`; placed in `app/` root
      ⬦🔐 (auth)/ ## [Route Group] Organizes routes or scopes layouts; omitted from the URL path
        ⬥🏛️ layout.tsx ## Scopes layout to subpages (also applies to `error.tsx` / `loading.tsx`, etc.) without exposing `(auth)` in the URL
        ⬦📁 login/
          ⬥🔑 page.tsx ## Maps to `/login`, not `/(auth)/login`
      ⬦📁 blog/
        ⬦🎯 [id]/ ## [Dynamic Route] Parses `/blog/1` as `params.id = '1'` for `page.tsx`
          ⬥📄 page.tsx
          ⬥🖼️ opengraph-image.png ## Social preview (static/dynamic via `.png`/`.jpg`/`.tsx`; also `twitter-image`)
      ⬦📁 docs/
        ⬦🎯 [...slug]/ ## [Catch-all Route] Matches `/docs/a` or `/docs/a/b` (use `[[...slug]]` to also match `/docs`)
          ⬥📄 page.tsx
      ⬦📁 dashboard/ ## (Folder used to demonstrate Parallel Routes)
        ⬥🔲 layout.tsx ## [Slot Container] Accepts `@analytics` as the `analytics` prop
        ⬥📊 page.tsx
        ⬥🛟 default.tsx ## [Default Fallback] Shown on hard navigation to paths in `@analytics` but not in main route
      ⬦🪟 @analytics/ ## [Parallel Slot] Navigates like a shadow in sync with main route, but loading and errors never interfere
          ⬥📈 page.tsx ## Matches `a/b/` when main route is at `dashboard/a/b/`
          ⬥🛟 default.tsx ## [Slot Fallback] Shown on hard navigation to paths in main route but missing here
      ⬦📁 feed/ ## (Folder used to demonstrate Intercepting Routes)
        ⬥🔲 layout.tsx ## [Slot Container] Accepts `@modal` as the `modal` prop
        ⬥📱 page.tsx
        ⬦📁 photo/
          ⬦🎯 [id]/
            ⬥📄 page.tsx ## Hard navigation bypasses interception and renders this page directly
        ⬦🪟 @modal/
          ⬦📸 (.)photo/ ## Intercepts via `(.)` same / `(..)` parent / `(..)(..)` 2-up / `(...)` root (relative to URL, not folder)
            ⬦🎯 [id]/ ## Soft navigation to `/feed/photo/1` matches `(.)photo/[id]`
              ⬥📄 page.tsx ## Intercepts `/feed/photo/1/page.tsx` and renders as a modal
          ⬥🛟 default.tsx ## [Slot Fallback] Returns null when no modal is triggered to keep it hidden
      ⬦📁 api/
        ⬦📁 users/
          ⬥🔌 route.ts ## [API Endpoint] Defines GET/POST for `/api/users` (cannot share folder with `page.tsx`)
      ⬦🧩 _components/ ## [Private Folder] `_` prefix opts out of routing; stores shared components (buttons, navbars)
        ⬦🧱 Button.tsx
    ⬥📡 instrumentation.ts ## [Server Telemetry] Runs at server startup; sets up tracing and backend metrics
    ⬥📡 instrumentation-client.ts ## [Client Telemetry] Runs post-load before interaction; tracks load errors and frontend performance
    ⬥📝 mdx-components.tsx ## [MDX Mapping] Requires @next/mdx; maps Markdown elements to custom React components
    ⬥🛡️ proxy.ts ## [Global Proxy] Intercepts requests for redirects, rewrites, and headers (replaces `middleware.ts`)
  ⬥📦 public/ ## [Static Assets] Serves images/fonts; accessed via `/logo.png`, not `/public/logo.png`
    ⬦🖼️ logo.png
  ⬥⚙️ next.config.ts ## [Global Config] Configures image domains, server rewrites, and build rules
```
