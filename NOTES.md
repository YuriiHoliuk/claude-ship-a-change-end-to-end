# Notes

## Plan
Added `PUT /users/:id`. I explored `routes/users.js`, `db/store.js` and `tests/update-user.test.js` first, then wrote a plan: add an `updateUser` helper to the store, add a route that validates input, returns 404 for unknown users, and calls the store. I kept POST validation unchanged to keep the diff small.

## Model choice
Sonnet 5.5. The change is small and follows existing patterns, so a faster, cheaper model fits.

## Commit split
1. `store: add updateUser`
2. `users: add PUT /users/:id with validation`
3. `docs: add NOTES.md`

## What review caught
Validation uses `typeof === "string"` plus a non-empty trim check, which is stricter than POST's truthiness check (POST would accept `name: 123`). A non-numeric id becomes `NaN`, finds no user, and returns 404 without crashing. Validation runs before the lookup, so a bad body gets 400 even for an unknown id.
