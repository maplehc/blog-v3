# 项目目录结构指南 (Project Structure Guide)

这份文档旨在为开发团队提供清晰的项目结构全景视图，明确各文件夹及文件的职责边界、相互关系以及维护规范，帮助新成员快速上手并保持项目的一致性。

## 1. 整体目录结构概览

项目采用 **Nuxt 4** 架构模式，结合 **Nuxt Content** 进行静态内容管理，核心逻辑主要位于 `app/` 目录下。

```text
blog-v3/
├── .vscode/                # IDE 配置文件
├── app/                    # 前端应用核心逻辑 (Nuxt App)
│   ├── assets/             # 样式与静态图标
│   ├── components/         # Vue 组件库 (按功能分类)
│   ├── composables/        # 可复用的组合式函数 (Hooks)
│   ├── pages/              # 路由视图文件 (基于文件路由)
│   ├── plugins/            # Nuxt 插件
│   ├── stores/             # Pinia 状态管理
│   ├── types/              # TypeScript 类型定义
│   └── utils/              # 纯函数工具库
├── content/                # Markdown 内容数据 (文章、预览、静态页)
├── docs/                   # 项目文档与指南 (本目录)
├── modules/                # 自定义 Nuxt 模块
├── patches/                # 依赖库补丁
├── public/                 # 根目录静态资源
├── remark-plugins/         # Markdown 渲染引擎插件 (Remark/Rehype)
├── scripts/                # 自动化运维脚本
├── server/                 # 后端 API 与 Nitro 服务器路由
├── shared/                 # 前后端共享代码
├── blog.config.ts          # 站点全局配置文件 (核心)
├── nuxt.config.ts          # Nuxt 框架底层配置
├── content.config.ts       # 内容集合与 Schema 定义
├── app.config.ts           # 应用 UI 与功能配置
└── package.json            # 依赖管理与执行脚本
```

---

## 2. 核心目录详细说明

### 2.1 根目录一级文件夹

| 文件夹     | 命名规范  | 设计意图与职责                                                                             |
| :--------- | :-------- | :----------------------------------------------------------------------------------------- |
| `app/`     | `app`     | **应用层**。包含所有前端 UI、交互逻辑和页面。Nuxt 4 的标准应用目录。                       |
| `content/` | `content` | **数据层**。存放所有以 Markdown 格式存在的博文、静态页面内容。由 Nuxt Content 管理。       |
| `server/`  | `server`  | **服务端**。Nitro 服务引擎的 API 接口 (`api/`) 和直接路由 (`routes/`)。                    |
| `scripts/` | `scripts` | **工具链**。包含项目初始化、新建文章、Feed 检查等自动化脚本。                              |
| `modules/` | `modules` | **扩展层**。存放针对本项目开发的本地 Nuxt 模块（如防镜像模块）。                           |
| `public/`  | `public`  | **资源层**。存放无需构建、直接映射到根路径的静态资源（如 Favicon、Fonts、Atom.xml 样式）。 |
| `shared/`  | `shared`  | **共享层**。存放前端 (`app/`) 和后端 (`server/`) 都能调用的通用工具函数。                  |

### 2.2 关键子文件夹递归说明

#### `app/` (前端应用核心)
*   **`assets/`**: 包含全局 SCSS 样式。
    *   `css/`: `_variable.scss` (变量), `main.scss` (入口), `article.scss` (文章专场样式)。
    *   `icons/`: 本地存放的 SVG 图标。
*   **`components/`**: 按业务逻辑划分。
    *   `blog/`: 博客框架组件（Header, Footer, Sidebar, Panel）。
    *   `content/`: 专门用于 Markdown 渲染的自定义 ProSe 组件。
    *   `post/`: 文章列表、摘要、评论、导航等组件。
    *   `widget/`: 侧边栏小部件（日志、统计、技术栈、目录）。
    *   `partial/`: 通用 UI 基础组件（Button, Pagination, Toggle）。前缀为 `Z`。
*   **`pages/`**: 
    *   `index.vue`: 首页。
    *   `[...slug].vue`: 动态文章渲染页。
    *   `archive.vue`: 归档页面。
    *   `link.vue`: 友链页面。

#### `content/` (内容管理)
*   **`posts/`**: 正式发布的博文，按 `年/月/文件名.md` 组织。
*   **`previews/`**: 预览状态的文章，不计入正式列表。
*   **`link.md`**: 友链页面的数据源。
*   **`theme.md`**: 主题演示页面数据。

---

## 3. 核心配置文件与脚本

| 文件名                    | 用途                                                                     | 加载时机    | 修改注意事项                                                |
| :------------------------ | :----------------------------------------------------------------------- | :---------- | :---------------------------------------------------------- |
| `blog.config.ts`          | **站点核心配置**。作者信息、站点标题、分类定义、脚本注入。               | 构建/启动时 | 换主、修改分类、添加全局统计代码时必改。                    |
| `nuxt.config.ts`          | **框架配置**。模块引入、Vite 插件、SEO 默认头、路由规则。                | 开发/构建时 | 仅限修改框架底层逻辑或引入新 Nuxt 模块时。                  |
| `app.config.ts`           | **UI/UX 配置**。侧边栏链接、页脚版权、组件默认属性（如代码块折叠行数）。 | 运行时      | 修改导航菜单、页脚信息、调整 UI 交互细节时。                |
| `content.config.ts`       | **内容 Schema 定义**。定义文章元数据（Frontmatter）的 Zod 校验规则。     | 内容解析时  | 新增文章元数据字段（如 `recommend`, `draft`）时需同步更新。 |
| `scripts/new-blog.ts`     | **新建文章脚本**。通过交互式命令行快速创建符合规范的 MD 文件。           | 手动执行    | `pnpm new` 调用。                                           |
| `scripts/init-project.ts` | **项目初始化脚本**。用于快速设置新克隆的项目环境。                       | 手动执行    | `pnpm init-project` 调用。                                  |
| `wrangler.toml`           | **Cloudflare 部署配置**                                                  | 部署时      | 修改 KV 绑定、自定义域名等。                                |
| `eslint.config.mjs`       | **代码规范配置**                                                         | 开发/校验时 | 调整 Lint 规则。                                            |
| `stylelint.config.mjs`    | **样式规范配置**                                                         | 开发/校验时 | 调整 SCSS 规范。                                            |
| `cspell.json`             | **拼写检查配置**                                                         | 校验时      | 添加专业术语白名单。                                        |

---

## 4. 版本控制与 CI/CD 规范

### 4.1 版本控制策略 (.gitignore)
*   **输出目录**: `.output/`, `.nuxt/`, `.nitro/`, `dist/` 均被忽略。
*   **依赖目录**: `node_modules/` 被忽略。
*   **敏感信息**: `.env` 文件被忽略。
*   **特殊路径**: `content/posts/test.md` 被排除，防止测试文章上线。

### 4.2 读写权限与产物输出
*   **构建产物**: `pnpm generate` 生成的静态文件位于 `dist/` 目录。
*   **服务端渲染产物**: `pnpm build` 生成的 Nitro Server 位于 `.output/` 目录。
*   **目录权限**: `content/` 目录通常由内容运营者频繁写入，而 `app/` 仅由开发者操作。

---

## 5. 可维护性与变更要求

1.  **同步更新**: 任何新增或删除一级文件夹的操作，必须在此文档中同步更新说明。
2.  **代码评审**: 结构性变更（如重命名 `components/` 下的子文件夹）必须通过代码评审（PR），且评审者需确认此文档已更新。
3.  **引用一致性**: `README.md` 必须始终包含指向此文档的有效链接。
4.  **100% 覆盖**: 交付的任何功能模块若涉及新目录结构，需在提交信息中体现对结构的补充描述。

---

## 6. 开发者快速定位指南

*   **我想改首页逻辑**: 前往 `app/pages/index.vue`。
*   **我想改文章页样式**: 前往 `app/assets/css/article.scss` 或修改 `app/components/content/` 下的组件。
*   **我想加一个新的导航链接**: 修改 `app.config.ts` 中的 `nav` 数组。
*   **我想加一篇新博文**: 运行 `pnpm new`。
*   **我想自定义代码高亮**: 修改 `app/shiki.config.ts`。
