# Tech Stack

## Backend
- PHP 8.2+
- Laravel 11
- MySQL/MariaDB or SQLite (testing)
- Laravel Passport (API authentication)
- Livewire 3.x (reactive components)

## Frontend
- AdminLTE 2.x (Bootstrap 3 theme)
- jQuery, Select2, Bootstrap Table
- Laravel Mix (Webpack wrapper)
- LESS for stylesheets

## Key Dependencies
- `watson/validating` - Model validation trait
- `intervention/image` - Image manipulation
- `tecnickcom/tcpdf` - PDF generation
- `bacon/bacon-qr-code` - QR code generation
- `onelogin/php-saml` - SAML authentication
- `spatie/laravel-backup` - Database backups
- `league/csv` - CSV import/export

## Common Commands

```bash
# Install dependencies
composer install
npm install

# Build frontend assets
npm run dev          # Development build
npm run production   # Production build (minified)
npm run watch        # Watch for changes

# Database
php artisan migrate
php artisan db:seed

# Testing
php artisan test                              # Run all tests
php artisan test tests/Unit/AccessoryTest.php # Single file
php artisan test --group=ldap                 # Run group
php artisan test --exclude-group=ldap         # Exclude group

# Code quality
./vendor/bin/phpstan analyse                  # Static analysis
./vendor/bin/phpcs                            # Code sniffer

# Cache management
php artisan config:cache
php artisan route:cache
php artisan view:cache
```

## Environment Configuration
- Copy `.env.example` to `.env`
- For testing: copy `.env.testing.example` to `.env.testing`
- Key settings: `APP_KEY`, `DB_*`, `MAIL_*`, `APP_URL`
