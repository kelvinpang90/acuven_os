# Acuven Technology â€” Landing Site

å…¬å¸å®˜ç½‘ï¼Œå•é¡µé™æ€ç«™ï¼ˆself-contained `index.html` + å†…è” CSS/JSï¼‰ã€‚

## å¿«é€Ÿå¼€å§‹

1. **æ”¹å“ç‰Œ**ï¼šè·Ÿ [REBRAND.md](REBRAND.md) ä¸€é”®æ›¿æ¢å ä½ç¬¦ï¼ˆå…¬å¸å / åŸŸå / é‚®ç®± / WhatsApp ç­‰ï¼‰ã€‚
2. **æœ¬åœ°é¢„è§ˆ**ï¼š`python -m http.server 8000` â†’ http://localhost:8000
3. **éƒ¨ç½²**ï¼šè·Ÿ [deploy/README.md](deploy/README.md) é… GitHub Secrets + VPS åˆå§‹åŒ–ï¼Œä¹‹åŽ `git push origin main` è‡ªåŠ¨éƒ¨ç½²ã€‚

## æŠ€æœ¯é€‰åž‹

- **é›¶æž„å»º**ï¼šå•æ–‡ä»¶ HTMLï¼Œå†…è” CSS + åŽŸç”Ÿ JSã€‚`<5 ç§’`ä»Ž git push åˆ°ä¸Šçº¿
- **i18n**ï¼šEN â‡„ ZH é€šè¿‡ `data-zh` å±žæ€§ + JS åˆ‡æ¢ï¼Œæ— ä¾èµ–
- **å“åº”å¼**ï¼šç§»åŠ¨ç«¯ / å¹³æ¿ / æ¡Œé¢é€‚é…
- **SEO**ï¼šå®Œæ•´ meta æ ‡ç­¾ã€Open Graphã€Twitter Card
- **éƒ¨ç½²**ï¼šGitHub Actions â†’ rsync â†’ è‡ªæ‰˜ç®¡ Nginx VPS

## æ–‡ä»¶ç»“æž„

```
â”œâ”€â”€ index.html                  â† æ•´ä¸ªç½‘ç«™ï¼ˆself-containedï¼‰
â”œâ”€â”€ public/                     â† é™æ€èµ„æºï¼ˆfaviconã€og å›¾ç­‰ï¼Œå¯é€‰ï¼‰
â”œâ”€â”€ .github/workflows/
â”‚   â””â”€â”€ deploy.yml              â† CI/CD pipelineï¼ˆpush â†’ rsync â†’ smoke testï¼‰
â”œâ”€â”€ deploy/
â”‚   â”œâ”€â”€ README.md               â† VPS ç«¯åˆå§‹åŒ– + secrets é…ç½®è¯´æ˜Ž
â”‚   â””â”€â”€ nginx/
â”‚       â””â”€â”€ acuventech.com.conf  â† nginx server_block
â”œâ”€â”€ REBRAND.md                  â† å ä½ç¬¦ä¸€è§ˆ + ä¸€é”®æ›¿æ¢è„šæœ¬
â””â”€â”€ README.md                   â† æœ¬æ–‡ä»¶
```

## åŽç»­å¯æ‰©å±•

å½“å‰æ˜¯ MVPã€‚åŽç»­å¦‚éœ€å¯è¡¥ï¼š

- **About / Team / Careers åŒºå—**ï¼šå†…å®¹è¡¥å…¨åŽå¢žåŠ  section
- **çœŸå®žæˆªå›¾æ›¿æ¢ mockup**ï¼šShowcase åŒºç›®å‰æ˜¯ div æ¨¡æ‹Ÿï¼Œå¯æ¢ä¸ºäº§å“çœŸå®žæˆªå›¾
- **è¡¨å•æ”¶é›†**ï¼šè”ç³»è¡¨å•ï¼ˆå¯ç”¨ web3forms / formspreeï¼Œå…åŽç«¯ï¼‰
- **Cookie / Analytics**ï¼šPlausibleã€Cloudflare Web Analytics ç­‰æ— ä¾µå…¥æ–¹æ¡ˆ
- **Sitemap / Robots**ï¼šSEO ä¸¥è°¨åŒ–æ—¶åŠ 
