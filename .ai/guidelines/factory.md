# Factory

These are the commands the `/factory` agents rely on. Every factory job runs in its own git worktree, which starts as a fresh checkout containing only committed files.

## Fresh Checkout Setup

Run these from the worktree root before doing anything else. They install dependencies, create `.env` and the SQLite database, run migrations and build the frontend assets:

```bash
touch database/database.sqlite
composer run setup
```

## Dev Server

- Start the server in the background on the port you were given: `php artisan serve --port=<port>`. The app is then at `http://127.0.0.1:<port>`.
- Do not use `composer run dev` or `npm run dev` in a worktree. They use fixed ports that clash between parallel jobs.
- Assets are served from the `npm run build` output. After changing CSS, JS or Blade files that use new Tailwind classes, run `npm run build` again before looking at the page.
- Stop the server when you are done.

## Validation

- `composer test`: runs Pint in check mode, PHPStan and the full test suite. Everything must pass before a build is ready.
- `php artisan test --compact --filter=<name>`: runs a narrower set of tests while iterating.
- `vendor/bin/pint --dirty --format agent`: fixes formatting in changed PHP files.
- `npm run build`: confirms the frontend compiles.
