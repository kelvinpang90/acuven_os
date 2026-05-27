# Deployment — acuventech.com

> ⚠️ 真实公司名/域名请按 [REBRAND.md](../REBRAND.md) 批量替换占位符后再部署。

静态站（单个 self-contained `index.html`），部署到自托管 VPS。

```
GitHub push main
   ↓
GitHub Actions (.github/workflows/deploy.yml)
   ├─ stage: cp index.html + public/ → stage/
   └─ rsync stage/  ──ssh──▶  VPS:/srv/sites/acuventech.com/
                                ↓
                         /srv/infra nginx
                         acuventech.com.conf → root /srv/sites/acuventech.com
                                ↓
                      https://acuventech.com
```

---

## 一、首次 VPS 端初始化（只做一次）

```bash
# 1. 建静态文件目录
sudo mkdir -p /srv/sites/acuventech.com
sudo chown <DEPLOY_USER>:<DEPLOY_GROUP> /srv/sites/acuventech.com

# 2. 确保 /srv/sites 已挂入 infra_nginx 容器（如已挂跳过）
#    在 /srv/infra/docker-compose.yml 的 nginx volumes 加一行：
#      - /srv/sites:/srv/sites:ro
#    然后：cd /srv/infra && sudo docker compose up -d nginx

# 3. 复制 nginx server_block
sudo cp deploy/nginx/acuventech.com.conf /srv/infra/nginx/conf.d/

# 4. 校验 + reload
sudo docker exec infra_nginx nginx -t
sudo docker exec infra_nginx nginx -s reload
```

> 通配符 / Origin 证书路径假设跟 `*.kelvinpeng.com` 模式相同。若不同，改 [acuventech.com.conf](nginx/acuventech.com.conf) 里 `ssl_certificate` 两行。

---

## 二、GitHub Secrets 配置

到 `https://github.com/<YOUR_ORG>/<YOUR_REPO>/settings/secrets/actions` 加 5–6 个 secret：

| Secret name        | 值（示例）                                                        | 说明 |
|--------------------|--------------------------------------------------------------------|-----|
| `SSH_HOST`         | VPS 域名或 IP                                                      | VPS 地址 |
| `SSH_USER`         | `ubuntu` / `deploy`                                                | SSH 登录用户 |
| `SSH_PORT`         | `22`                                                               | SSH 端口 |
| `SSH_PRIVATE_KEY`  | `-----BEGIN OPENSSH PRIVATE KEY-----` 起始的整段私钥内容           | GHA 用来 SSH 进 VPS |
| `DEPLOY_PATH`      | `/srv/sites/acuventech.com/`                                     | 目标目录，**末尾斜杠不能省** |
| `SITE_URL` *(可选)* | `https://acuventech.com`                                         | 部署后 smoke test 的 URL；不设则跳过测试 |

> 与 profile_os / erp_os / crm_os 用同一台 VPS 时，可复用同一组 SSH secrets，只需新增 `DEPLOY_PATH` 和 `SITE_URL`。

---

## 三、触发部署

```bash
git push origin main
```

或在 GitHub Actions 页面点 "Run workflow"（已开启 workflow_dispatch）。

---

## 四、回滚

**A. 回滚到上一个 git commit**：
```bash
git revert HEAD && git push   # 自动触发重新部署
```

**B. 文件级快照**（推荐加进 cron）：
```bash
# 部署前先在 VPS 上备份
sudo rsync -a /srv/sites/acuventech.com/ /srv/sites/backups/acuventech.com-$(date +%F-%H%M)/
# 回滚
sudo rsync -a --delete /srv/sites/backups/acuventech.com-2026-05-15-1430/ /srv/sites/acuventech.com/
```

---

## 五、本地预览

页面是 self-contained HTML，无需任何构建：

```bash
# 任选一种简单 HTTP server
python -m http.server 8000      # http://localhost:8000
# 或
npx serve .                     # http://localhost:3000
```

> 直接双击 `index.html` 也行，只是字体 preconnect 等需要 HTTP 协议才能完全生效。
