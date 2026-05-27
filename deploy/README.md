# Deployment â€” acuventech.com

> âš ï¸ çœŸå®žå…¬å¸å/åŸŸåè¯·æŒ‰ [REBRAND.md](../REBRAND.md) æ‰¹é‡æ›¿æ¢å ä½ç¬¦åŽå†éƒ¨ç½²ã€‚

é™æ€ç«™ï¼ˆå•ä¸ª self-contained `index.html`ï¼‰ï¼Œéƒ¨ç½²åˆ°è‡ªæ‰˜ç®¡ VPSã€‚

```
GitHub push main
   â†“
GitHub Actions (.github/workflows/deploy.yml)
   â”œâ”€ stage: cp index.html + public/ â†’ stage/
   â””â”€ rsync stage/  â”€â”€sshâ”€â”€â–¶  VPS:/srv/sites/acuventech.com/
                                â†“
                         /srv/infra nginx
                         acuventech.com.conf â†’ root /srv/sites/acuventech.com
                                â†“
                      https://acuventech.com
```

---

## ä¸€ã€é¦–æ¬¡ VPS ç«¯åˆå§‹åŒ–ï¼ˆåªåšä¸€æ¬¡ï¼‰

```bash
# 1. å»ºé™æ€æ–‡ä»¶ç›®å½•
sudo mkdir -p /srv/sites/acuventech.com
sudo chown <DEPLOY_USER>:<DEPLOY_GROUP> /srv/sites/acuventech.com

# 2. ç¡®ä¿ /srv/sites å·²æŒ‚å…¥ infra_nginx å®¹å™¨ï¼ˆå¦‚å·²æŒ‚è·³è¿‡ï¼‰
#    åœ¨ /srv/infra/docker-compose.yml çš„ nginx volumes åŠ ä¸€è¡Œï¼š
#      - /srv/sites:/srv/sites:ro
#    ç„¶åŽï¼šcd /srv/infra && sudo docker compose up -d nginx

# 3. å¤åˆ¶ nginx server_block
sudo cp deploy/nginx/acuventech.com.conf /srv/infra/nginx/conf.d/

# 4. æ ¡éªŒ + reload
sudo docker exec infra_nginx nginx -t
sudo docker exec infra_nginx nginx -s reload
```

> é€šé…ç¬¦ / Origin è¯ä¹¦è·¯å¾„å‡è®¾è·Ÿ `*.kelvinpeng.com` æ¨¡å¼ç›¸åŒã€‚è‹¥ä¸åŒï¼Œæ”¹ [acuventech.com.conf](nginx/acuventech.com.conf) é‡Œ `ssl_certificate` ä¸¤è¡Œã€‚

---

## äºŒã€GitHub Secrets é…ç½®

åˆ° `https://github.com/<YOUR_ORG>/<YOUR_REPO>/settings/secrets/actions` åŠ  5â€“6 ä¸ª secretï¼š

| Secret name        | å€¼ï¼ˆç¤ºä¾‹ï¼‰                                                        | è¯´æ˜Ž |
|--------------------|--------------------------------------------------------------------|-----|
| `SSH_HOST`         | VPS åŸŸåæˆ– IP                                                      | VPS åœ°å€ |
| `SSH_USER`         | `ubuntu` / `deploy`                                                | SSH ç™»å½•ç”¨æˆ· |
| `SSH_PORT`         | `22`                                                               | SSH ç«¯å£ |
| `SSH_PRIVATE_KEY`  | `-----BEGIN OPENSSH PRIVATE KEY-----` èµ·å§‹çš„æ•´æ®µç§é’¥å†…å®¹           | GHA ç”¨æ¥ SSH è¿› VPS |
| `DEPLOY_PATH`      | `/srv/sites/acuventech.com/`                                     | ç›®æ ‡ç›®å½•ï¼Œ**æœ«å°¾æ–œæ ä¸èƒ½çœ** |
| `SITE_URL` *(å¯é€‰)* | `https://acuventech.com`                                         | éƒ¨ç½²åŽ smoke test çš„ URLï¼›ä¸è®¾åˆ™è·³è¿‡æµ‹è¯• |

> ä¸Ž profile_os / erp_os / crm_os ç”¨åŒä¸€å° VPS æ—¶ï¼Œå¯å¤ç”¨åŒä¸€ç»„ SSH secretsï¼Œåªéœ€æ–°å¢ž `DEPLOY_PATH` å’Œ `SITE_URL`ã€‚

---

## ä¸‰ã€è§¦å‘éƒ¨ç½²

```bash
git push origin main
```

æˆ–åœ¨ GitHub Actions é¡µé¢ç‚¹ "Run workflow"ï¼ˆå·²å¼€å¯ workflow_dispatchï¼‰ã€‚

---

## å››ã€å›žæ»š

**A. å›žæ»šåˆ°ä¸Šä¸€ä¸ª git commit**ï¼š
```bash
git revert HEAD && git push   # è‡ªåŠ¨è§¦å‘é‡æ–°éƒ¨ç½²
```

**B. æ–‡ä»¶çº§å¿«ç…§**ï¼ˆæŽ¨èåŠ è¿› cronï¼‰ï¼š
```bash
# éƒ¨ç½²å‰å…ˆåœ¨ VPS ä¸Šå¤‡ä»½
sudo rsync -a /srv/sites/acuventech.com/ /srv/sites/backups/acuventech.com-$(date +%F-%H%M)/
# å›žæ»š
sudo rsync -a --delete /srv/sites/backups/acuventech.com-2026-05-15-1430/ /srv/sites/acuventech.com/
```

---

## äº”ã€æœ¬åœ°é¢„è§ˆ

é¡µé¢æ˜¯ self-contained HTMLï¼Œæ— éœ€ä»»ä½•æž„å»ºï¼š

```bash
# ä»»é€‰ä¸€ç§ç®€å• HTTP server
python -m http.server 8000      # http://localhost:8000
# æˆ–
npx serve .                     # http://localhost:3000
```

> ç›´æŽ¥åŒå‡» `index.html` ä¹Ÿè¡Œï¼Œåªæ˜¯å­—ä½“ preconnect ç­‰éœ€è¦ HTTP åè®®æ‰èƒ½å®Œå…¨ç”Ÿæ•ˆã€‚
