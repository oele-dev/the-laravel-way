# View file names: kebab-case or camelCase

Boost names a view file in kebab-case: `show-filtered.blade.php`. Spatie names it in camelCase: `openSource.blade.php`. The framework writes kebab-case everywhere I looked.

## What the sources say

**Laravel Boost** says kebab-case:

> View | kebab-case | `show-filtered.blade.php`

Source: [`rules/style.md` in laravel/boost](https://github.com/laravel/boost/blob/cbd7f56a2cae7de07138aa96a5863319ddf11a0b/.ai/laravel/skill/laravel-best-practices/rules/style.md)

**Spatie** says camelCase:

> View files use camelCase: `openSource.blade.php`

Source: [`spatie-laravel-php-guidelines.md` in spatie/guidelines-skills](https://github.com/spatie/guidelines-skills/blob/a9c9d1a3174c1b1693c025f82c71dac4601de4b2/resources/boost/skills/spatie-laravel-php/references/spatie-laravel-php-guidelines.md)

## What the framework does

Kebab-case.

**The docs.** I searched every `view('...')` call in the Laravel 13 docs. Twelve view names have more than one word, and all twelve use hyphens: `auth.forgot-password`, `auth.two-factor-challenge`, `errors.invalid-order`, `livewire.pulse.top-sellers`. None is camelCase or snake_case.

Source: [laravel/docs at the commit I read](https://github.com/laravel/docs/tree/2e7f6c6d3c), for example [`passwords.md`, line 94](https://github.com/laravel/docs/blob/2e7f6c6d3c/passwords.md#L94)

**The generators.** `make:component` builds the view path by running `Str::kebab()` on each segment:

```php
return (new Collection($path))
    ->map(fn ($segment) => Str::kebab($segment))
    ->implode('.');
```

Source: [`ComponentMakeCommand` in laravel/framework](https://github.com/laravel/framework/blob/c33d357a57/src/Illuminate/Foundation/Console/ComponentMakeCommand.php#L134-L142)

`make:view` is neutral. It keeps the name you type and only turns dots and backslashes into slashes, so `make:view openSource` creates `openSource.blade.php` without complaint.

Source: [`ViewMakeCommand::getNameInput()` in laravel/framework](https://github.com/laravel/framework/blob/c33d357a57/src/Illuminate/Foundation/Console/ViewMakeCommand.php)

**The starter kits.** The Livewire starter kit ships `forgot-password.blade.php`, `two-factor-challenge.blade.php` and `delete-user-form.blade.php`.

Source: [`resources/views` in laravel/livewire-starter-kit](https://github.com/laravel/livewire-starter-kit/tree/52bce9a263/resources/views)

## Verdict

I use kebab-case: `open-source.blade.php`.

The framework and Boost agree, so there is nothing to argue. My own agent rules said camelCase. That line came from Spatie's guidelines, and Laravel does the opposite in its docs, its generator and its starter kits. I am changing my rules.

One thing to watch: `make:view` will not correct you. It creates whatever name you type, so the convention has to be written in the project's agent rules.

In a codebase that already uses camelCase views I leave them alone. Renaming a view means touching every `view()` call and every `@include` that names it, and it buys nothing.

## Example

```
resources/views/auth/forgot-password.blade.php
```

```php
return view('auth.forgot-password');
```

---

Sources read on 2026-11-11.
