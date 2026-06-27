# FlecBlog 主题模板

基于 `@flecblog/core-nuxt` 的主题开发模板。

## 快速开始

### 1. 创建主题仓库

点击 GitHub 上的 **Use this template** 按钮，创建你的主题仓库。

建议仓库名格式：`flec-theme-xxx`（如 `flec-theme-minimal`、`flec-theme-dark`）

### 2. 克隆到本地

```bash
git clone https://github.com/你的用户名/你的仓库名.git my-theme
cd my-theme
```

### 3. 安装依赖

```bash
npm install
```

### 4. 启动后端服务（Docker Compose）

主题开发需要后端 API 服务。用 Docker Compose 一键启动数据库 + API + 管理后台：

```bash
docker compose up -d
```

启动后：
- **管理后台**：http://localhost:4000
- **API 服务**：http://localhost:8080

首次启动会自动创建演示数据（文章、分类、标签、评论等），开箱即用。

### 5. 配置 API 地址

```bash
cp .env.example .env
```

`.env.example` 已预设 `http://localhost:8080/api/v1`，指向 Docker Compose 启动的后端。

### 6. 启动主题开发

```bash
npm run dev
```

访问 http://localhost:3000 查看效果。

主题通过 `http://localhost:8080/api/v1` 连接真实后端，所有功能都可正常使用。

### 停止后端服务

```bash
docker compose down
```

如需清理数据（重置演示数据）：

```bash
docker compose down -v
```

---

## 开发指南

### 目录结构

```
my-theme/
├── app/
│   ├── app.vue              # 根组件
│   ├── error.vue            # 错误页
│   ├── layouts/             # 布局
│   ├── pages/               # 页面
│   └── assets/css/          # 样式
├── public/                  # 静态资源
├── theme.json               # 主题配置 schema
├── nuxt.config.ts           # Nuxt 配置
├── .env.example             # 环境变量示例
├── docker-compose.yml       # 后端服务（PostgreSQL + API + 管理后台）
├── Dockerfile               # Docker 构建文件
└── .github/workflows/       # CI/CD 工作流
```

### 修改主题信息

编辑 `theme.json` 中的 `$meta`：

> ⚠️ `slug` 必须唯一，请先到 [主题中心](https://hub.flec.top/themes) 排查是否已被占用。

```json
{
  "$meta": {
    "slug": "your-theme-slug",
    "name": "你的主题名",
    "version": "1.0.0",
    "author": "你的名字",
    "description": "主题描述"
  }
}
```

### 添加配置项

在 `theme.json` 中添加配置字段，管理后台会自动生成表单：

```json
{
  "外观设置": {
    "accent_color": {
      "type": "string",
      "title": "强调色",
      "format": "color",
      "default": "#1a73e8"
    }
  }
}
```

在组件中读取配置：

```vue
<script setup>
const { themeConfig } = useTheme()
const accentColor = computed(() => themeConfig.value.accent_color)
</script>
```

### 添加菜单

在 `theme.json` 的 `$menus` 中定义：

```json
{
  "$menus": {
    "navigation": {
      "label": "导航菜单",
      "maxDepth": 2,
      "defaults": [
        { "title": "首页", "url": "/", "icon": "ri-home-line", "sort": 1, "is_enabled": true }
      ]
    }
  }
}
```

在组件中获取菜单：

```vue
<script setup>
const { getMenus } = useTheme()
const navMenus = getMenus('navigation')
</script>
```

### 功能开关

通过 `$features` 控制主题支持的功能：

```json
{
  "$features": {
    "moments": true,
    "feedback": true,
    "oauth": true,
    "site_subscribe": true
  }
}
```

### 可用的 Composables

无需 import，直接使用：

| 函数 | 说明 |
|---|---|
| `useTheme()` | 主题配置和菜单 |
| `useSysConfig()` | 系统配置 |
| `useArticleList()` | 文章列表 |
| `useArticle(slug)` | 文章详情 |
| `useCurrentArticle()` | 当前文章（跨页面共享） |
| `useComments()` | 评论 |
| `useAuth()` | 认证状态 |
| `useDarkMode()` | 暗色模式 |
| `useCategories()` | 分类 |
| `useTags()` | 标签 |
| `useToast()` | 消息提示 |
| `renderMarkdown()` | 渲染 Markdown |
| `formatDate()` | 格式化日期 |

详见 [core-nuxt 使用指南](./docs/01-目录与快速上手.md)

---

## 构建镜像

### 1. 配置仓库权限

进入仓库 **Settings → Actions → General → Workflow permissions**，选择 **Read and write permissions**。

### 2. 推送标签触发构建

```bash
git add .
git commit -m "feat: 初始版本"
git tag v1.0.0
git push origin main --tags
```

### 3. 查看构建结果

在 GitHub 仓库的 **Actions** 页面查看构建进度。

构建成功后，镜像会推送到 GitHub Container Registry：

```
ghcr.io/你的用户名/你的仓库名:1.0.0
```

### 手动触发构建

也可以在 GitHub Actions 页面手动触发，需要填写版本号。

---

## 许可证

MIT
