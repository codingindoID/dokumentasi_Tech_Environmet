# Source

link :

```bash
https://filamentphp.com/docs/4.x/resources/overview
```

# Installasi ( v4 )

install via composer - saat dibuat menggunakan v4:

```bash
composer require filament/filament:"^4.0"

```

lalu create panel nya :

```bash
php artisan filament:install --panels

```

# make user filament

```bash
php artisan make:filament-user

```

# Create Model + migration

dalam contoh ini saya menggunakan model SLider, buat dulu model dan migrationnya :

```bash
php artisan make:model Slider --migration
```

jangan lupa setup migrationnya, contoh seperti ini , sesuaikan saja :

```php
 public function up(): void
    {
        Schema::create('sliders', function (Blueprint $table) {
            $table->id();
            $table->string('title');
            $table->string('descriptions');
            $table->string('image');
            $table->timestamps();
        });
    }
```

jalankan migrate nya :

```bash
php artisan migrate
```

jangan lupa setting fillable nya, pada `models/Slider.php`:

```php
 protected $fillable = ['title', 'descriptions', 'image'];
```

# Create Resource

buat resource filamennya :

```bash
php artisan make:filament-resource Slider --generate

```

nanti akan dibuatkan skema :

```bash
Slider/
├── SliderResource.php
├── Pages/
│   ├── CreateSlider.php
│   ├── EditSlider.php
│   └── ListSliders.php
├── Schemas/
│   └── SliderForm.php
└── Tables/
    └── SlidersTable.php
```

# Case Khusus Jika Ada Upload Image

karena contoh menggunakan image , ini direkomendasikan agar file lama otomatis terhapus jika ada file baru upload / delete,
edit pada `SliderForm.php` :

```php
 FileUpload::make('image')
                    ->image()
                    ->disk('public')
                    ->directory('sliders')
                    ->preserveFilenames()
                    ->deleteUploadedFileUsing(fn() => true)
                    ->afterStateUpdated(function ($state, $record) {
                        if ($record?->image && $record->image !== $state) {
                            Storage::disk('public')->delete($record->image);
                        }
                    })
                    ->required(),
```

baris ini :

```php
->preserveFilenames()
->deleteUploadedFileUsing(fn() => true)
->afterStateUpdated(function ($state, $record) {
    if ($record?->image && $record->image !== $state) {
        Storage::disk('public')->delete($record->image);
    }
})
```

kemudian pada `models/Slider.php` tambahkan baris ini:

```php
 protected static function booted()
    {
        static::deleting(function ($slider) {
            if ($slider->image) {
                Storage::disk('public')->delete($slider->image);
            }
        });
    }
```

# Link Storage ( Umum Di Laravel Jika menggunakan File)

```bash
php artisan storage:link

```
