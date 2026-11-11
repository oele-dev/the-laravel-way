# Route names: snake_case, camelCase or kebab-case

Everyone agrees on dots between segments: `photos.index`. The fight is over a segment with two words. Boost writes it in snake_case, Spatie in camelCase, and Laravel's own docs and starter kits use neither. They write kebab-case.

## What the sources say

**Laravel Boost** says snake_case inside a segment:

> Route name | Dotted segments, with snake_case within a segment when needed | `users.show_active`

Source: [`rules/style.md` in laravel/boost](https://github.com/laravel/boost/blob/cbd7f56a2cae7de07138aa96a5863319ddf11a0b/.ai/laravel/skill/laravel-best-practices/rules/style.md)

**Spatie** says camelCase:

> Route names: camelCase (`->name('openSource')`)

Source: [`spatie-laravel-php-guidelines.md` in spatie/guidelines-skills](https://github.com/spatie/guidelines-skills/blob/a9c9d1a3174c1b1693c025f82c71dac4601de4b2/resources/boost/skills/spatie-laravel-php/references/spatie-laravel-php-guidelines.md)

## What the framework does

Kebab-case, in the three places I checked.

**The docs.** I searched every `->name()` call in the Laravel 13 docs. No route name is written in camelCase or snake_case. Three route names have two words, and all three use a hyphen:

```php
Route::view('/checkout/success', 'checkout.success')->name('checkout-success');
Route::view('/checkout/cancel', 'checkout.cancel')->name('checkout-cancel');
```

The third is `download-invoice`, in the Cashier Paddle chapter.

Source: [`billing.md`, lines 277 and 278](https://github.com/laravel/docs/blob/2e7f6c6d3c/billing.md#L277-L278)

**Resource routes.** The registrar builds a route name from the resource string exactly as you typed it, plus a dot and the action:

```php
return trim(sprintf('%s%s.%s', $prefix, $name, $method), '.');
```

By that code, `Route::resource('photo-comments', ...)` names its routes `photo-comments.index`, `photo-comments.store` and so on. A kebab-case URI gives a kebab-case name with no extra work.

Source: [`ResourceRegistrar::getResourceRouteName()` in laravel/framework](https://github.com/laravel/framework/blob/c33d357a57/src/Illuminate/Routing/ResourceRegistrar.php#L677-L698)

**The starter kits.** The React starter kit names a route `user-password.update`, and both kits have `well-known.passkeys`.

Source: [`routes/settings.php` in laravel/react-starter-kit](https://github.com/laravel/react-starter-kit/blob/012f4248f7/routes/settings.php#L28)

So on this one the framework sides with neither guideline.

## Verdict

I use kebab-case inside a segment and dots between segments: `user-password.update`.

My agent rules said camelCase, from Spatie. Boost says snake_case. The docs and the starter kits use neither, and the framework comes first in my order. I am changing my rules.

There is a practical reason too. `Route::resource()` copies the resource string into the route name, and my URIs are kebab-case. With any other style, the names I write by hand would not match the names Laravel generates, unless I passed `names` to every resource.

This one goes against Boost, so the project's agent rules have to say kebab-case or the agent will write snake_case.

In a codebase that already names its routes another way I leave them alone. A route name is a string that views, redirects, tests and the frontend all repeat, and a rename has to find every one.

## Example

```php
Route::get('/open-source', [OpenSourceController::class, 'index'])->name('open-source');

Route::resource('photo-comments', PhotoCommentsController::class);
// photo-comments.index, photo-comments.store, photo-comments.show, ...
```

---

Sources read on 2026-11-11.
