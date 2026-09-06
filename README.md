# Omnia Registry

The list of modules and themes that can be installed from inside OmniCore.

## For users

You don't need anything here. OmniCore reads this list for you —
open the admin face and go to **Marketplace**.

## For anyone submitting a module or theme

1. Push your module or theme to a public GitHub repo. A module needs
   an `index.js` at its root; a theme needs an `index.html`.
2. Get the commit hash you want listed: `git rev-parse HEAD`
3. Open a pull request adding one entry to `registry.json`.

```json
{
	"id": "my-module",
	"name": "My Module",
	"description": "One line about what it does",
	"repo": "https://github.com/you/your-repo",
	"ref": "the-commit-hash",
	"author": "your-github-username",
	"minOmniCore": "1.1.0"
}
```

`id` becomes the folder name it installs to, so it must be lowercase
letters, numbers and dashes only.

### `minOmniCore`

The oldest OmniCore version your module or theme actually works on —
not a version of your own. Nothing here has a version number; the
commit in `ref` **is** the version, and pointing `ref` at a new commit
**is** how you ship an update. `minOmniCore` only answers a narrower
question: what's the minimum OmniCore someone needs for this commit to
work at all?

Treated as "this major version, this release or later" — so `1.1.0`
means anything from 1.1.0 up through the rest of the 1.x line, but not
2.0.0. You never need to name an upper bound: a future breaking major
version is already a separate, deliberate decision on the OmniCore
side, not something this field has to anticipate.

Get this right, because it's what OmniCore checks before installing or
silently updating anything:

- Didn't change what core features you rely on? Leave it exactly as it
  was in your last accepted PR.
- Started using something new — a data type OmniCore only just
  started emitting, say — bump it to the version that introduced that.
  Get this wrong (too low) and someone's install can crash on your
  update instead of skipping it.

### One repo, several themes or modules

If your repo holds more than one — a studio publishing ten themes from
a single repo, say — add `path`, pointing at the subfolder for that
one entry:

```json
{
	"id": "metro",
	"name": "Metro",
	"description": "Windows-8 style tiles",
	"repo": "https://github.com/your-studio/themes",
	"path": "themes/metro",
	"ref": "the-commit-hash",
	"author": "your-studio"
}
```

Each theme or module in the repo gets its own entry with its own `path`.
Only what's inside that folder gets installed — the rest of the repo is
left behind. Leave `path` out entirely when your repo holds just the one
thing; that's still the common case.

### Why a commit hash and not a branch

OmniCore downloads exactly the commit listed here. If this pointed at a
branch, code could change after review and everyone would silently get
the new version. Pinning means any change needs its own pull request,
and gets looked at before it reaches anybody.

### Updating your entry

Open another pull request changing the `ref`. Same review, same reason.
That alone is enough to ship an update — but if the new commit needs a
newer OmniCore than your current `minOmniCore` promises, raise that
field in the same PR. An update whose `minOmniCore` a person's install
doesn't meet is skipped rather than installed, so getting this right
is what keeps an update from reaching someone it would actually break.
