# Docker Laravel

A Docker development environment for Laravel applications with PHP 8.2, MySQL, and Nginx.

## 📋 Requirements

- Docker 20.x or higher
- Docker Compose 2.x or higher

## 🏗️ Project Structure

```bash
docker-laravel/
├── docker/
│   ├── mysql/
│   │   └── my.cnf              # MySQL configuration
│   ├── nginx/
│   │   └── default.conf        # Nginx virtual host
│   └── php/
│       ├── Dockerfile          # PHP-FPM image
│       └── php.ini             # PHP configuration
├── src/                        # Laravel application root
│   └── ...
├── .dockerignore
├── .gitignore
├── docker-compose.yml
└── README.md
```

## 🚀 Tech Stack

- **PHP**: 8.2-FPM
- **Composer**: 2.x (latest)
- **Laravel**: 12.x
- **MySQL**: 8.0
- **Nginx**: Latest
- **Node.js**: 22.x LTS
- **npm**: Latest

## 🛠️ Quick Start

### New Laravel Project

```bash
# Start containers
docker compose up -d --build

# Enter container
docker compose exec app bash
```

Inside container:

The working directory is already `/var/www/html/src` (empty on first run). Choose one of the following.

Option 1 — Using Composer

```bash
composer create-project laravel/laravel . "12.*" --prefer-dist

cd src
cp .env.example .env
php artisan key:generate

# match docker-compose values
# DB_HOST=db
# DB_DATABASE=laravel_db
# DB_USERNAME=laravel_user
# DB_PASSWORD=laravel_password

php artisan migrate
```

Option 2 — Using the Laravel Installer

```bash
composer global require laravel/installer
"$(composer global config bin-dir --absolute)"/laravel new . --no-interaction

cp .env.example .env
php artisan key:generate
php artisan migrate
```

## 🌐 Access URLs

- **Application**: <http://localhost>

## 🗄️ Database Information

**From within containers:**

- Host: `db`
- Port: `3306`
- Database: `laravel_db`
- Username: `laravel_user`
- Password: `laravel_password`
- Root Password: `root_password`

> **Note**: Database is not exposed to host machine. Access only available from within Docker network.

## 🔧 Development

### File Permissions (Linux/macOS)

```bash
# Fix storage and cache permissions
docker compose exec app chown -R www-data:www-data storage bootstrap/cache
```

### Database Connection

Update your `.env`:

```env
DB_CONNECTION=mysql
DB_HOST=db
DB_PORT=3306
DB_DATABASE=laravel_db
DB_USERNAME=laravel_user
DB_PASSWORD=laravel_password
```

## 💡 Tips

Common Commands:

```bash
# Container management
docker compose up -d          # Start containers
docker compose down           # Stop containers
docker compose logs -f app    # View logs

# Enter container for multiple commands
docker compose exec app bash

# Laravel commands (inside container)
php artisan migrate
php artisan make:controller
php artisan tinker

# Frontend commands (inside container)
npm run dev
npm run build
npm run watch

# Or run single commands from outside
docker compose exec app php artisan migrate
docker compose exec app composer install
```
