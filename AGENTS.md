# AGENTS.md

## Stack
Rails 8.1.3, SQLite3 (storage/development.sqlite3), ERB templates, Hotwire (turbo-rails + stimulus-rails), Importmap, Propshaft asset pipeline, Jbuilder for JSON views, Minitest + Capybara/Selenium for system tests. No background job framework.

## Commands
- Setup: bin/setup
- Run dev server: bin/dev
- Run tests: bin/rails test
- Lint: bundle exec rubocop

## Conventions
- Controllers respond with HTML by default; use Turbo Streams for partial page updates
- Shared partials live in app/views/shared/
- Use bin/rails generate for boilerplate, never hand-write migrations or models
- Strong parameters required in every controller

## Donts
- No new gems without approval
- No inline JavaScript in ERB files
- No skip_before_action :verify_authenticity_token
- Do not seed real user data; use db/seeds.rb only
- Do not string-interpolate SQL; use parameterized queries
