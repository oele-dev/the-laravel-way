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

I use the plural: `PhotosController`. That is a departure from the Laravel way, and I make it on purpose.

A model is one row, so it is singular and `$user->name` reads right. A controller is not a row. It answers for the resource, and everywhere else Laravel names the resource in the plural: the table is `photos`, the URI is `/photos`, the route names are `photos.index` and `photos.destroy`. I read the singular controller as the model's naming rule carried over to a class that is not a model. The generator does exactly that: it takes the model name and appends `Controller`.

Rails made the same call. Its guide says "Rails favors making the resource in the controller's name plural."

Source: [Action Controller Overview, Rails Guides](https://guides.rubyonrails.org/action_controller_overview.html)

The departure has a cost:

- `make:model Photo --controller` and `make:model Photo --all` create `PhotoController`. I generate the controller on its own instead.
- Boost tells agents to write the singular. The same table says to "preserve an established project convention", so the project's rules have to state the plural or the agent will not know.

I keep the singular when there is no collection: a singleton resource such as `ProfileController`, and single-action controllers.

In a codebase that already uses singular names I leave them alone. A rename buys nothing.

## Example

```bash
php artisan make:model Photo --migration
php artisan make:controller PhotosController --model=Photo --resource
```

```php
// routes/web.php
use App\Http\Controllers\PhotosController;

Route::resource('photos', PhotosController::class);
```

The table, the URI and the controller are plural. The model is singular.

---

Sources read on 2026-11-11.
