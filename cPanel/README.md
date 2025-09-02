1. Clone the repo on the server

```bash
# SSH into server (or use cPanel Terminal)
ssh username@server.example.com

cd /home/username
git clone https://github.com/your-account/your-repo.git project
# or use the SSH URL if you set deploy keys: git@github.com:your-account/your-repo.git
```

2. Point domain to `public`
- In cPanel Domains, set the document root to `/home/username/project/public`.
- If you can't change it, create a symlink:

```bash
ln -s /home/username/project/public /home/username/public_html/project
```

3. First-time setup (run once)

```bash
cd /home/username/project
composer install --no-dev --optimize-autoloader
cp .env.example .env
# edit .env (DB, APP_URL, etc.) using nano, vim, or cPanel File Manager
php artisan key:generate
find . -type f -exec chmod 644 {} \;
find . -type d -exec chmod 755 {} \;
chmod -R 775 storage bootstrap/cache
php artisan migrate --force
php artisan storage:link
php artisan config:cache
php artisan route:cache
php artisan view:cache
```