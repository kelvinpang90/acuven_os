# Acuven Technology — Landing Site

公司官网，单页静态站（self-contained `index.html` + 内联 CSS/JS）。

## 快速开始

1. **改品牌**：跟 [REBRAND.md](REBRAND.md) 一键替换占位符（公司名 / 域名 / 邮箱 / WhatsApp 等）。
2. **本地预览**：`python -m http.server 8000` → http://localhost:8000
3. **部署**：跟 [deploy/README.md](deploy/README.md) 配 GitHub Secrets + VPS 初始化，之后 `git push origin main` 自动部署。

## 技术选型

- **零构建**：单文件 HTML，内联 CSS + 原生 JS。`<5 秒`从 git push 到上线
- **i18n**：EN ⇄ ZH 通过 `data-zh` 属性 + JS 切换，无依赖
- **响应式**：移动端 / 平板 / 桌面适配
- **SEO**：完整 meta 标签、Open Graph、Twitter Card
- **部署**：GitHub Actions → rsync → 自托管 Nginx VPS

## 文件结构

```
├── index.html                  ← 整个网站（self-contained）
├── public/                     ← 静态资源（favicon、og 图等，可选）
├── .github/workflows/
│   └── deploy.yml              ← CI/CD pipeline（push → rsync → smoke test）
├── deploy/
│   ├── README.md               ← VPS 端初始化 + secrets 配置说明
│   └── nginx/
│       └── acuventech.com.conf  ← nginx server_block
├── REBRAND.md                  ← 占位符一览 + 一键替换脚本
└── README.md                   ← 本文件
```

## 后续可扩展

当前是 MVP。后续如需可补：

- **About / Team / Careers 区块**：内容补全后增加 section
- **真实截图替换 mockup**：Showcase 区目前是 div 模拟，可换为产品真实截图
- **表单收集**：联系表单（可用 web3forms / formspree，免后端）
- **Cookie / Analytics**：Plausible、Cloudflare Web Analytics 等无侵入方案
- **Sitemap / Robots**：SEO 严谨化时加
