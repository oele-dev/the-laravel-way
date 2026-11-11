# Controller names: singular or plural

Laravel names a resource controller after the singular model: `PhotoController`. Spatie names it after the plural resource: `PhotosController`. Both rules ship as agent guidelines today, so an agent with both installed gets two answers.

## What the sources say

**Laravel Boost** says singular. Its naming table has the row:

> Controller | Singular resource name | `ArticleController`

Source: [`rules/style.md` in laravel/boost](https://github.com/laravel/boost/blob/cbd7f56a2cae7de07138aa96a5863319ddf11a0b/.ai/laravel/skill/laravel-best-practices/rules/style.md)

**Spatie** says plural:

> Use plural resource names (`PostsController`)

Its controller section ends with "Treat this as a loose guideline rather than a rule that must always be enforced."

Source: [`spatie-laravel-php-guidelines.md` in spatie/guidelines-skills](https://github.com/spatie/guidelines-skills/blob/a9c9d1a3174c1b1693c025f82c71dac4601de4b2/resources/boost/skills/spatie-laravel-php/references/spatie-laravel-php-guidelines.md)

## What the framework does

Singular, in both places I checked.

**The docs.** The controllers chapter for Laravel 13 never uses a plural controller name. `PhotoController` appears 26 times and `UserController` 22. The resource route pairs a plural URI with a singular class:

```php
Route::resource('photos', PhotoController::class);
```

Source: [Controllers, Laravel 13.x](https://laravel.com/docs/13.x/controllers) ([file at the commit I read](https://github.com/laravel/docs/blob/95a3dd5a0375bb69e5e3833b660636708f919765/controllers.md))

**The generator.** `make:model` with `--controller` builds the controller name from the model name and appends `Controller`:

```php
$controller = Str::studly(class_basename($this->argument('name')));

$this->call('make:controller', array_filter([
    'name' => "{$controller}Controller",
```

So `php artisan make:model Photo --controller` asks for `PhotoController`.

Source: [`ModelMakeCommand::createController()` in laravel/framework](https://github.com/laravel/framework/blob/c9f577068b8660a2ea8b1f20974cdc44f36bf420/src/Illuminate/Foundation/Console/ModelMakeCommand.php#L160-L167)

`make:controller` on its own takes whatever name you type. Nothing in the framework rejects a plural name.

## Verdict

<!-- Osmell writes this section: which one he uses, and when he would break it. -->

## Example

```bash
php artisan make:model Photo --controller --resource
```

```php
// routes/web.php
use App\Http\Controllers\PhotoController;

Route::resource('photos', PhotoController::class);
```

The URI is plural, the model is singular, and the controller follows the model.

---

Sources read on 2026-11-11.
