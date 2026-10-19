# Omnia Registry

The list of modules and themes that can be installed from inside OmniCore.

## For users

You don't need anything here. OmniCore reads this list for you: open the
admin face and go to **Marketplace**.

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
	"license": "MIT",
	"minOmniCore": "1.1.0"
}
```

`id` becomes the folder name it installs to, so it must be lowercase
letters, numbers and dashes only.

Read [the rules](#the-rules) before you open the pull request.

### `license`

The licence you give your module or theme, as its
[SPDX name](https://spdx.org/licenses/): `MIT`, `0BSD`, `Apache-2.0`,
`GPL-3.0-only` and so on. The commit you list should have the licence
in it too, in a `LICENSE` file.

Your module or theme is your own work, not part of OmniCore, so the
licence is yours to choose: open or closed, free or paid (see OmniCore's
[LICENSING.md](https://github.com/tanzim2000/Omnia-OmniCore/blob/main/LICENSING.md),
"Resources are their own works"). The Marketplace shows it on your
listing's page, under Source, before anyone installs.

New entries need it. Older entries without it still install, and show
"No licence stated".

### `minOmniCore`

The oldest OmniCore version your module or theme actually works on, not
a version of your own. Nothing here has a version number: the commit in
`ref` **is** the version, and pointing `ref` at a new commit **is** how
you ship an update. `minOmniCore` only answers a narrower question: what's
the minimum OmniCore someone needs for this commit to work at all?

Treated as "this major version, this release or later", so `1.1.0`
means anything from 1.1.0 up through the rest of the 1.x line, but not
2.0.0. You never need to name an upper bound: a future breaking major
version is already a separate, deliberate decision on the OmniCore
side, not something this field has to anticipate.

Get this right, because it's what OmniCore checks before installing or
silently updating anything:

- Didn't change what core features you rely on? Leave it exactly as it
  was in your last accepted PR.
- Started using something new (a data type OmniCore only just started
  emitting, say)? Bump it to the version that introduced that. Get this
  wrong (too low) and someone's install can crash on your update instead
  of skipping it.

### What your module does beyond its tile

Three more fields, each one `true` or left out. They don't change how
anything runs: they're what the Marketplace shows people before they
install, as tags on the listing and a sentence each on its page. A
reviewer checks them against the module, so be honest:

| Field        | Set it to `true` when your module...                                                                                  |
| ------------ | --------------------------------------------------------------------------------------------------------------------- |
| `background` | keeps running the whole time OmniCore is on. It's the same as `"background": true` in your `module.json`.             |
| `inport`     | takes input: it has an `input.json`, so the face it's on gets an Inport with controls for it.                         |
| `notifies`   | can pop up notifications on the screens showing its face.                                                             |

```json
{
	"id": "ntfy",
	"minOmniCore": "1.19.0",
	"background": true,
	"notifies": true
}
```

Only an exact `true` counts. Leave a field out rather than setting it to
`false`. They live here rather than in your `module.json` because this
file is the part that gets reviewed: a module can't hide that it runs in
the background by not mentioning it. The Marketplace shows them from
OmniCore 1.19.6; an older OmniCore ignores them.

`minOmniCore` is shown too, on every listing, as "OmniCore 1.19.0 or
newer", and in red when the OmniCore looking at it is older than that.

### One repo, several themes or modules

If your repo holds more than one (a studio publishing ten themes from a
single repo, say), add `path`, pointing at the subfolder for that one
entry:

```json
{
	"id": "metro",
	"name": "Metro",
	"description": "Windows-8 style tiles",
	"repo": "https://github.com/your-studio/themes",
	"path": "themes/metro",
	"ref": "the-commit-hash",
	"author": "your-studio",
	"license": "MIT"
}
```

Each theme or module in the repo gets its own entry with its own `path`.
Only what's inside that folder gets installed; the rest of the repo is
left behind. Leave `path` out entirely when your repo holds just the one
thing; that's still the common case.

### Why a commit hash and not a branch

OmniCore downloads exactly the commit listed here. If this pointed at a
branch, code could change after review and everyone would silently get
the new version. Pinning means any change needs its own pull request,
and gets looked at before it reaches anybody.

### Updating your entry

Open another pull request changing the `ref`. Same review, same reason.
That alone is enough to ship an update. But if the new commit needs a
newer OmniCore than your current `minOmniCore` promises, raise that
field in the same PR. An update whose `minOmniCore` a person's install
doesn't meet is skipped rather than installed, so getting this right
is what keeps an update from reaching someone it would actually break.

## The rules

A pull request that breaks one of these isn't merged, and an entry found
breaking one later is taken out.

1. **List only your own work.** The `repo` must be yours, or one you're
   allowed to list for its owner, and `author` must be you (or them).
2. **A fork says it's a fork.** If your module or theme is built on
   someone else's, say so in its `description` ("A fork of ... by ..."),
   credit the original in your repo, and follow the original's licence.
   It needs to be different enough to be worth its own entry.
3. **No re-listing someone else's work under your own name,** whatever
   its licence allows. A copy with a new name and a new author is not a
   new module.
4. **One entry per thing.** Don't list the same module or theme twice
   under different names or ids.
5. **Be honest in the fields.** `background`, `inport`, `notifies`,
   `minOmniCore` and `license` are checked in review.
6. **No "official".** Don't call a module or theme "official", or say
   or suggest it's made or approved by the Omnia project, and don't name
   it after Omnia, OmniCore or the other names in OmniCore's
   [TRADEMARKS.md](https://github.com/tanzim2000/Omnia-OmniCore/blob/main/TRADEMARKS.md).
   "For OmniCore" or "works with OmniCore" is fine.

By opening a pull request that adds or changes an entry, you agree that
the entry itself (its id, name, description and the other fields) is
published in this list under the list's licence below. Your module or
theme keeps its own licence.

## Licence

The list (`registry.json` and this README) is under the
[Creative Commons Attribution 4.0 International](LICENSE) licence
(CC BY 4.0). You may copy it, mirror it, and build your own list from it,
as long as you credit the OmniCore Resource Registry, link to the
licence, and say if you changed anything.

When you mirror it, keep each entry's `author`, `repo` and `ref` as they
are: they're how people know whose work an entry is. A mirror may say
it's a mirror of the OmniCore Resource Registry; it may not call itself
the official registry.

The modules and themes the list points to are not covered by this
licence. Each one is under the licence its author gave it (its `license`
field, and the `LICENSE` file in its repo).
