# Next.js 文件路由系统详解

## 概述

Next.js 的文件路由系统是基于**文件系统约定**（File System Convention）实现的，这是一种创新的路由管理方式，让开发者无需手动配置路由表，而是通过文件结构自动生成路由。

## 底层实现原理

### 1. 文件系统扫描
Next.js 在构建时会扫描 `pages/` 或 `app/` 目录，分析文件结构：
- 读取所有 `.js`、`.jsx`、`.ts`、`.tsx` 文件
- 解析文件夹层级关系
- 生成对应的路由映射表

### 2. 路由映射算法
```javascript
// 简化的路由映射逻辑
function generateRoutes(filePath) {
  // 移除文件扩展名
  const cleanPath = filePath.replace(/\.(js|jsx|ts|tsx)$/, '');
  
  // 处理 index 文件
  if (cleanPath.endsWith('/index')) {
    return cleanPath.replace('/index', '') || '/';
  }
  
  // 处理动态路由
  if (cleanPath.includes('[') && cleanPath.includes(']')) {
    return cleanPath.replace(/\[([^\]]+)\]/g, ':$1');
  }
  
  return cleanPath;
}
```

### 3. 构建时优化
- **代码分割**：每个页面自动生成独立的 JavaScript 包
- **预渲染**：支持静态生成（SSG）和服务端渲染（SSR）
- **路由预取**：自动预取链接页面的资源

## Pages Router（传统方式）

### 基本规则
```
pages/
├── index.js          → /
├── about.js          → /about
├── blog/
│   ├── index.js      → /blog
│   └── [slug].js     → /blog/:slug
└── api/
    └── users.js      → /api/users
```

### 特殊文件
- `_app.js` - 应用级组件
- `_document.js` - HTML 文档结构
- `_error.js` - 错误页面
- `404.js` - 404 页面

## App Router（新方式，推荐）

### 目录结构
```
app/
├── layout.tsx        → 根布局
├── page.tsx          → 首页 (/)
├── about/
│   └── page.tsx      → /about
├── blog/
│   ├── layout.tsx    → 博客布局
│   ├── page.tsx      → /blog
│   └── [slug]/
│       └── page.tsx  → /blog/:slug
└── api/
    └── users/
        └── route.ts  → /api/users
```

### 特殊文件
- `layout.tsx` - 布局组件
- `page.tsx` - 页面组件
- `loading.tsx` - 加载状态
- `error.tsx` - 错误边界
- `not-found.tsx` - 404 页面
- `route.ts` - API 路由

## 动态路由

### Pages Router
```javascript
// pages/post/[id].js
export default function Post({ params }) {
  return <div>Post ID: {params.id}</div>;
}

// pages/post/[...slug].js (捕获所有路由)
export default function Post({ params }) {
  return <div>Slug: {params.slug.join('/')}</div>;
}
```

### App Router
```javascript
// app/post/[id]/page.tsx
export default function Post({ params }: { params: { id: string } }) {
  return <div>Post ID: {params.id}</div>;
}

// app/post/[...slug]/page.tsx
export default function Post({ params }: { params: { slug: string[] } }) {
  return <div>Slug: {params.slug.join('/')}</div>;
}
```

## 高级功能

### 1. 嵌套路由
```
app/
├── dashboard/
│   ├── layout.tsx    → 仪表板布局
│   ├── page.tsx      → /dashboard
│   ├── analytics/
│   │   └── page.tsx  → /dashboard/analytics
│   └── settings/
│       └── page.tsx  → /dashboard/settings
```

### 2. 并行路由
```
app/
├── @analytics/
│   └── page.tsx      → 并行路由
├── @team/
│   └── page.tsx      → 并行路由
└── dashboard/
    └── page.tsx      → 主路由
```

### 3. 拦截路由
```
app/
├── (..)photo/
│   └── [id]/
│       └── page.tsx  → 拦截路由
└── photo/
    └── [id]/
        └── page.tsx  → 原始路由
```

## 路由组

### 组织路由而不影响 URL
```
app/
├── (marketing)/
│   ├── about/
│   └── contact/
├── (shop)/
│   ├── products/
│   └── cart/
└── layout.tsx
```

## API 路由

### Pages Router
```javascript
// pages/api/users.js
export default function handler(req, res) {
  if (req.method === 'GET') {
    res.status(200).json({ users: [] });
  }
}
```

### App Router
```javascript
// app/api/users/route.ts
export async function GET() {
  return Response.json({ users: [] });
}

export async function POST(request: Request) {
  const body = await request.json();
  return Response.json({ created: true });
}
```

## 性能优化

### 1. 自动代码分割
- 每个页面自动生成独立的 JavaScript 包
- 按需加载，减少初始包大小

### 2. 预取策略
```javascript
// 自动预取
<Link href="/about">About</Link>

// 手动预取
import { prefetch } from 'next/router';
prefetch('/about');
```

### 3. 静态优化
- 静态页面自动预渲染
- 支持增量静态再生（ISR）

## 最佳实践

### 1. 文件命名
- 使用小写字母和连字符
- 避免特殊字符
- 保持简洁明了

### 2. 目录结构
- 合理组织文件层级
- 使用路由组进行逻辑分组
- 避免过深的嵌套

### 3. 性能考虑
- 合理使用动态导入
- 优化图片和资源加载
- 利用缓存策略

## 总结

Next.js 的文件路由系统通过文件系统约定实现了：
- **零配置**：无需手动配置路由
- **类型安全**：TypeScript 支持
- **性能优化**：自动代码分割和预取
- **灵活性**：支持各种复杂路由场景

这种设计让开发者可以专注于业务逻辑，而不用花费时间在路由配置上，大大提高了开发效率。

## 参考资料

- [Next.js 官方文档 - 路由](https://nextjs.org/docs/app/building-your-application/routing)
- [Next.js 官方文档 - Pages Router](https://nextjs.org/docs/pages/building-your-application/routing)
- [Next.js GitHub 仓库](https://github.com/vercel/next.js)
