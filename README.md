# Omnia Registry

The list of modules and themes that can be installed from inside OmniCore.

## For users

You don't need anything here. OmniCore reads this list for you —
open the admin face and go to **Marketplace**.

## For anyone submitting a module or theme

1. Push your module or theme to its own public GitHub repo.
   A module needs an `index.js` at the root; a theme needs an
   `index.html`.
2. Get the commit hash you want listed: `git rev-parse HEAD`
3. Open a pull request adding one entry to `registry.json`.

```json
{
	"id": "my-module",
	"name": "My Module",
	"description": "One line about what it does",
	"repo": "https://github.com/you/your-repo",
	"ref": "the-commit-hash",
	"author": "your-github-username"
}
```

`id` becomes the folder name it installs to, so it must be lowercase
letters, numbers and dashes only.

### Why a commit hash and not a branch

OmniCore downloads exactly the commit listed here. If this pointed at a
branch, code could change after review and everyone would silently get
the new version. Pinning means any change needs its own pull request,
and gets looked at before it reaches anybody.

### Updating your entry

Open another pull request changing the `ref`. Same review, same reason.
