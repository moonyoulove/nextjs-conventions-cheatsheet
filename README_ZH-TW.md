# nextjs-conventions-cheatsheet

> [English](./README.md) | 繁體中文

⬥ 表示名稱是約定不可更改；⬦ 表示名稱只是示例並不是固定的

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
  ⬥📁 src/ ## 能自動識別 `src/app/` 或 `app/`，並非必須資料夾
    ⬥📁 app/
      ⬥🏛️ layout.tsx ## ＜Layout＞ [共用外框] 在此排版側邊欄和頁尾元件 (根目錄下的 `layout.tsx` 必須返回 ＜html＞ ＜body＞)
      ⬥🔄 template.tsx ## ⠀＜Template＞ [動態外框] 與 `layout.tsx` 類似，但換頁時強制重新掛載以重置狀態
      ⬥🚨 error.tsx ## ⠀⠀＜ErrorBoundary＞ 發生錯誤時改成顯示此頁面 (必須宣告 'use client')
      ⬥⏳ loading.tsx ## ⠀⠀⠀＜Suspense＞ 基於 React Suspense 實作，抓取非同步資料時自動顯示骨架畫面
      ⬥🚫 not-found.tsx ## ⠀⠀⠀⠀＜NotFound＞ 網址不存在時顯示 (類似還有實驗中的 `forbidden.tsx`、`unauthorized.tsx`)
      ⬥🖥️ page.tsx ## ⠀⠀⠀⠀⠀＜Page＞ [核心畫面] 對應路由首頁 `/` (預設為 Server Component)
      ⬥💥 global-error.tsx ## 根目錄 `layout.tsx` 發生錯誤時顯示 (必須宣告 'use client' 並返回 ＜html＞ ＜body＞)
      ⬥🌐 favicon.ico ## 包括 `icon`、`apple-icon` (支援 `.ico`/`.png`/`.tsx` 等；僅 `favicon.ico` 放在 `app/` 根目錄)
      ⬥🗺️ sitemap.xml ## 支援 `.xml`/`.ts`，放在 `app/` 根目錄
      ⬥🤖 robots.txt ## [爬蟲規則] 支援 `.txt`/`.ts`，放在 `app/` 根目錄
      ⬥📱 manifest.json ## [Web App 清單] 支援 `.json`/`.ts`，放在 `app/` 根目錄
      ⬦🔐 (auth)/ ## [路由群組] 用於組織資料夾或切分獨立版型，網址中直接省略不呈現
        ⬥🏛️ layout.tsx ## 無需在網址暴露 `(auth)` 路徑，為子頁面提供專屬版型 (同理適用於 `error.tsx` / `loading.tsx` 等)
        ⬦📁 login/
          ⬥🔑 page.tsx ## 對應網址為 `/login` 而不是 `/(auth)/login`
      ⬦📁 blog/
        ⬦🎯 [id]/ ## [動態路由] 將 `/blog/1` 解析為 params.id = '1' 並傳給 `page.tsx`
          ⬥📄 page.tsx
          ⬥🖼️ opengraph-image.png ## 社群預覽圖 (支援 `.png`/`.jpg`/`.tsx` 等動靜態生成；另有 `twitter-image`)
      ⬦📁 docs/
        ⬦🎯 [...slug]/ ## [Catch-all 路由] 匹配 `/docs/a` 或 `/docs/a/b` (使用 `[[...slug]]` 則可以同時匹配根路徑 `/docs`)
          ⬥📄 page.tsx
      ⬦📁 dashboard/ ## (此資料夾用於解釋平行路由)
        ⬥🔲 layout.tsx ## [外框容器] `@analytics` 會作為 analytics 參數傳入此處排版
        ⬥📊 page.tsx
        ⬥🛟 default.tsx ## [預設備援] 硬導航至 `@analytics` 有但主路由沒有的路徑時顯示
      ⬦🪟 @analytics/ ## [平行插槽] 像影子一樣與主路由同步導航，但載入與錯誤互不干擾
          ⬥📈 page.tsx ## 主路由在 `dashboard/a/b/` 時，`@analytics` 會同步導航到 `a/b/`
          ⬥🛟 default.tsx ## [插槽備援] 硬導航至主路由有但 `@analytics` 沒有的路徑時顯示
      ⬦📁 feed/ ## (此資料夾用於解釋攔截路由)
        ⬥🔲 layout.tsx ## [外框容器] `@modal` 會作為 modal 參數傳入此處排版
        ⬥📱 page.tsx
        ⬦📁 photo/
          ⬦🎯 [id]/
            ⬥📄 page.tsx ## 硬導航時不會觸發攔截，會直接顯示此單獨頁面
        ⬦🪟 @modal/
          ⬦📸 (.)photo/ ## 使用 `(.)`同層 / `(..)`上一層 / `(..)(..)`上兩層 / `(...)`根層 設定攔截(路徑是相對於網址而不是資料夾)
            ⬦🎯 [id]/ ## 軟導航到 `/feed/photo/1` 時剛好與 `(.)photo/[id]` 匹配
              ⬥📄 page.tsx ## 攔截 `/feed/photo/1/page.tsx` 渲染，改以彈出式視窗呈現
          ⬥🛟 default.tsx ## [插槽備援] 沒觸發彈窗時返回 null 讓彈出式視窗不顯示
      ⬦📁 api/
        ⬦📁 users/
          ⬥🔌 route.ts ## [API 端點] 定義 `/api/users` 的 GET/POST 等方法 (不可以跟 UI 元件例如 `page.tsx` 放在同一個資料夾)
      ⬦🧩 _components/ ## [私有資料夾] 用 `_` 排除在路由之外 (未放 `page.tsx`/`route.ts` 的資料夾預設亦不會被公開)
        ⬦🧱 Button.tsx
    ⬥📡 instrumentation.ts ## [伺服端遙測監控] 服務啟動時執行，用於設定後端事件追蹤、指標
    ⬥📡 instrumentation-client.ts ## [客戶端遙測監控] 頁面載入後、使用者互動前執行，專門追蹤頁面載入錯誤與前端效能
    ⬥📝 mdx-components.tsx ## [MDX元件對應] 需搭配 @next/mdx，把 Markdown 標籤 (如 #、連結) 換成自訂的 React 元件
    ⬥🛡️ proxy.ts ## [全域代理] 請求抵達前先攔截檢查，處理重新導向、轉址與 Header (取代原有的 `middleware.ts`)
  ⬥📦 public/ ## [靜態資源] 存放圖片、字型，以 `/logo.png` 存取而不是 `/public/logo.png`
    ⬦🖼️ logo.png
  ⬥⚙️ next.config.ts ## [全域設定] 設定外部圖片來源、伺服器轉址與打包編譯規則
```
