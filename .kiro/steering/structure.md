# Project Structure

```
snipe-it/
├── app/
│   ├── Actions/          # Single-purpose action classes
│   ├── Console/Commands/ # Artisan CLI commands
│   ├── Enums/            # PHP enums (ActionType, etc.)
│   ├── Events/           # Domain events (checkout, checkin)
│   ├── Exceptions/       # Custom exception classes
│   ├── Helpers/          # Static helper utilities
│   ├── Http/
│   │   ├── Controllers/  # Web & API controllers (organized by domain)
│   │   ├── Middleware/   # Request middleware
│   │   ├── Requests/     # Form request validation
│   │   ├── Traits/       # Controller traits
│   │   └── Transformers/ # API response transformers
│   ├── Importer/         # CSV import logic per entity type
│   ├── Livewire/         # Livewire components
│   ├── Mail/             # Mailable classes
│   ├── Models/           # Eloquent models
│   │   └── Traits/       # Model traits (Searchable, Loggable, etc.)
│   ├── Notifications/    # Notification classes
│   ├── Observers/        # Model observers
│   ├── Policies/         # Authorization policies
│   ├── Presenters/       # View presenters (formatting)
│   ├── Providers/        # Service providers
│   ├── Rules/            # Custom validation rules
│   ├── Services/         # Business logic services
│   └── View/             # View components
├── config/               # Configuration files
├── database/
│   ├── factories/        # Model factories for testing
│   ├── migrations/       # Database migrations
│   └── seeders/          # Database seeders
├── public/               # Web root (assets, uploads)
├── resources/
│   ├── assets/           # Source JS/LESS files
│   ├── lang/             # Translation files
│   └── views/            # Blade templates
├── routes/
│   ├── api.php           # API routes
│   ├── web.php           # Web routes (loads web/*.php)
│   └── web/              # Modular web route files
├── storage/              # Logs, cache, uploads
└── tests/
    ├── Feature/          # Integration tests
    └── Unit/             # Unit tests
```

## Key Patterns

### Models
- Extend `SnipeModel` or `Depreciable` base classes
- Use `ValidatingTrait` for automatic validation on save
- Use traits: `Searchable`, `Loggable`, `CompanyableTrait`, `SoftDeletes`
- Define `$rules`, `$fillable`, `$casts` arrays
- Relationships use full namespace: `\App\Models\Company::class`

### Controllers
- Organized by domain in subdirectories (Assets/, Users/, etc.)
- Use Form Request classes for validation
- Authorize via policies: `$this->authorize('view', $asset)`
- Return views or `Helper::getRedirectOption()` for redirects

### API
- Routes in `routes/api.php`
- Controllers in `app/Http/Controllers/Api/`
- Transformers format responses
- Passport token authentication

### Testing
- Feature tests for HTTP endpoints
- Unit tests for model logic
- Use factories for test data
- SQLite in-memory for fast tests
