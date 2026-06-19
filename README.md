# Discord Tool v6

## تشغيل على الاستضافة

### متطلبات
- Node.js 20 أو أحدث

### خطوات التشغيل

1. ارفع محتوى هذا الملف المضغوط على السيرفر
2. انسخ ملف `.env.example` باسم `.env` وعدّل القيم إذا لزم
3. شغّل الأمر:

```bash
node index.mjs
```

أو باستخدام npm:

```bash
npm start
```

### متغيرات البيئة

| المتغير | القيمة الافتراضية | الوصف |
|---------|------------------|-------|
| `PORT` | `3000` | البورت اللي السيرفر بيشتغل عليه |
| `STATIC_DIR` | `./public` | مسار ملفات الواجهة |
| `NODE_ENV` | `production` | بيئة التشغيل |

### استخدام PM2 (موصى به)

```bash
npm install -g pm2
pm2 start index.mjs --name discord-tool
pm2 save
pm2 startup
```

### nginx (Reverse Proxy)

```nginx
server {
    listen 80;
    server_name yourdomain.com;

    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_cache_bypass $http_upgrade;
        # مهم لـ SSE (live streaming)
        proxy_buffering off;
        proxy_read_timeout 3600s;
    }
}
```
