# Next.js Dashboard Application

一个功能完整的 Next.js 仪表板应用程序，使用 App Router 构建，包含用户管理、客户管理、发票管理和收入统计功能。

## 🚀 功能特性

### 📊 仪表板功能
- **数据概览**: 显示发票总数、客户总数、已付/待付金额
- **收入图表**: 月度收入趋势可视化
- **最新发票**: 显示最近5条发票记录

### 👥 用户管理
- 用户注册和登录
- 密码加密存储
- 会话管理

### 🏢 客户管理
- 客户信息存储和管理
- 客户搜索和过滤
- 客户头像显示
- 客户发票统计

### 📄 发票管理
- 发票创建、编辑、删除
- 发票状态管理（已付/待付）
- 发票搜索和分页
- 发票与客户关联

### 📈 收入统计
- 月度收入数据存储
- 收入趋势分析
- 数据可视化

## 🛠️ 技术栈

### 前端技术
- **Next.js 15.4.7** - React 框架，使用 App Router
- **React 19** - 用户界面库
- **TypeScript 5.7.3** - 类型安全
- **Tailwind CSS 3.4.17** - 样式框架
- **Heroicons** - 图标库

### 后端技术
- **PostgreSQL** - 关系型数据库
- **NextAuth.js 5.0.0-beta.25** - 身份验证
- **bcryptjs** - 密码加密
- **postgres** - 数据库连接

### 开发工具
- **pnpm** - 包管理器
- **ESLint** - 代码检查
- **Prettier** - 代码格式化

## 🗄️ 数据库结构

### 表结构
1. **users** - 用户表
   - id (UUID, 主键)
   - name (VARCHAR)
   - email (TEXT, 唯一)
   - password (TEXT, 加密)

2. **customers** - 客户表
   - id (UUID, 主键)
   - name (VARCHAR)
   - email (VARCHAR)
   - image_url (VARCHAR)

3. **invoices** - 发票表
   - id (UUID, 主键)
   - customer_id (UUID, 外键)
   - amount (INT)
   - status (VARCHAR)
   - date (DATE)

4. **revenue** - 收入表
   - month (VARCHAR, 唯一)
   - revenue (INT)

## 🚀 快速开始

### 环境要求
- Node.js 18+ 
- PostgreSQL 数据库
- pnpm 包管理器

### 安装步骤

1. **克隆仓库**
   ```bash
   git clone https://github.com/liuliuliu-git/nextjs-dashboard.git
   cd nextjs-dashboard
   ```

2. **安装依赖**
   ```bash
   pnpm install
   ```

3. **环境配置**
   ```bash
   cp .env.example .env.local
   ```
   
   配置以下环境变量：
   ```env
   POSTGRES_URL=your_postgres_connection_string
   AUTH_SECRET=your_auth_secret
   AUTH_URL=http://localhost:3000/api/auth
   ```

4. **数据库初始化**
   ```bash
   # 访问种子数据端点
   curl http://localhost:3000/api/seed
   ```

5. **启动开发服务器**
   ```bash
   pnpm dev
   ```

6. **访问应用**
   打开浏览器访问 [http://localhost:3000](http://localhost:3000)

## 📁 项目结构

```
nextjs-dashboard/
├── app/                    # App Router 目录
│   ├── dashboard/          # 仪表板页面
│   │   ├── customers/     # 客户管理
│   │   ├── invoices/      # 发票管理
│   │   └── layout.tsx     # 仪表板布局
│   ├── lib/               # 工具库
│   │   ├── data.ts        # 数据库查询
│   │   ├── definitions.ts # 类型定义
│   │   └── utils.ts       # 工具函数
│   ├── ui/                # UI 组件
│   │   ├── dashboard/     # 仪表板组件
│   │   ├── customers/    # 客户组件
│   │   └── invoices/     # 发票组件
│   └── layout.tsx         # 根布局
├── public/                # 静态资源
├── package.json           # 项目配置
└── README.md             # 项目说明
```

## 🔧 开发命令

```bash
# 开发模式
pnpm dev

# 构建生产版本
pnpm build

# 启动生产服务器
pnpm start
```

## 📊 API 端点

- `GET /api/seed` - 初始化数据库数据
- `GET /api/query` - 查询接口（示例）

## 🎨 UI 组件

### 仪表板组件
- `Cards` - 数据卡片
- `LatestInvoices` - 最新发票列表
- `RevenueChart` - 收入图表
- `SideNav` - 侧边导航

### 表单组件
- `LoginForm` - 登录表单
- `CreateForm` - 创建发票表单
- `EditForm` - 编辑发票表单

### 表格组件
- `CustomersTable` - 客户表格
- `InvoicesTable` - 发票表格

## 🔒 安全特性

- 密码 bcrypt 加密
- SQL 注入防护
- 环境变量配置
- 类型安全验证

## 📱 响应式设计

- 移动端适配
- 平板端优化
- 桌面端完整功能

## 🤝 贡献指南

1. Fork 本仓库
2. 创建功能分支 (`git checkout -b feature/AmazingFeature`)
3. 提交更改 (`git commit -m 'Add some AmazingFeature'`)
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 创建 Pull Request

## 📄 许可证

本项目采用 MIT 许可证 - 查看 [LICENSE](LICENSE) 文件了解详情

## 🙏 致谢

- [Next.js](https://nextjs.org/) - React 框架
- [Tailwind CSS](https://tailwindcss.com/) - CSS 框架
- [Heroicons](https://heroicons.com/) - 图标库
- [PostgreSQL](https://www.postgresql.org/) - 数据库

## 📞 联系方式

如有问题或建议，请通过以下方式联系：

- GitHub Issues: [创建问题](https://github.com/liuliuliu-git/nextjs-dashboard/issues)
- Email: 3078421415@qq.com

---

⭐ 如果这个项目对您有帮助，请给它一个星标！