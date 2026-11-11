# Config file names: snake_case or kebab-case

Boost names a two-word config file in snake_case: `google_calendar.php`. Spatie names it in kebab-case: `pdf-generator.php`. The framework cannot break this tie, because it never ships a config file with two words in its name.

## What the sources say

**Laravel Boost** says snake_case:

> Configuration file | snake_case | `google_calendar.php`

Source: [`rules/style.md` in laravel/boost](https://github.com/laravel/boost/blob/cbd7f56a2cae7de07138aa96a5863319ddf11a0b/.ai/laravel/skill/laravel-best-practices/rules/style.md)

**Spatie** says kebab-case, and says to avoid the new file when it can:

> Files: kebab-case (`pdf-generator.php`)
>
> Add service configs to `config/services.php`, don't create new files

Source: [`spatie-laravel-php-guidelines.md` in spatie/guidelines-skills](https://github.com/spatie/guidelines-skills/blob/a9c9d1a3174c1b1693c025f82c71dac4601de4b2/resources/boost/skills/spatie-laravel-php/references/spatie-laravel-php-guidelines.md)

## What the framework does

It avoids the question.

**The framework.** Laravel 13 ships 16 config files and every name is one word: `app`, `auth`, `broadcasting`, `cache`, `concurrency`, `cors`, `database`, `filesystems`, `hashing`, `images`, `logging`, `mail`, `queue`, `services`, `session`, `view`.

Source: [`config/` in laravel/framework](https://github.com/laravel/framework/tree/c33d357a57/config)

**First-party packages.** I listed the `config` folder of 16 Laravel packages: Sanctum, Horizon, Telescope, Pulse, Reverb, Octane, Scout, Pennant, Cashier, Cashier Paddle, Fortify, Passport, Nightwatch, Boost, MCP and the AI SDK. Each ships one file with a one-word name. The package with two words in its name still picks one: `laravel/cashier-paddle` ships `config/cashier.php`.

Source: [`config/` in laravel/cashier-paddle](https://github.com/laravel/cashier-paddle/tree/82bfc68eb7/config)

**The docs.** The Laravel 13 docs never mention a file under `config/` with a two-word name.

So the first tiebreaker has nothing to say. What Laravel shows instead is a habit: find one word.

## Verdict

I use snake_case when I can't avoid the second word: `google_calendar.php`.

The framework gives no answer here, so the next source in my order decides, and that is Boost. My agent rules said kebab-case, from Spatie. I am changing them.

Before I name the file I try what Laravel does and look for one word. Credentials for a third-party service go in `config/services.php`, which is Spatie's advice and I keep it. A new file is for configuration that belongs to my own app.

snake_case also matches the keys inside the file. Laravel writes its own keys that way (`frontend_url`, `expire_on_close`), so a call like `config('google_calendar.api_key')` reads in one style from end to end.

## Example

```php
// config/google_calendar.php
return [
    'api_key' => env('GOOGLE_CALENDAR_API_KEY'),
];
```

```php
$key = config('google_calendar.api_key');
```

---

Sources read on 2026-11-11.
