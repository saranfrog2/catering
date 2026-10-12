# User template notes (Galascribe-internal)

This file, and the rest of `.user-template/`, is for Galascribe's own generation
step. It is not part of a generated publication repository: Galascribe
deletes `.user-template/` in the identity commit it makes immediately after
generating a new repository from this user template, so a person writing an
article never sees it.

## Placeholders Galascribe must rewrite

Every value below is schema-valid as shipped (so the user template passes
`.user-template/scripts/verify-user-template.mjs` unmodified), but it is a stand-in.
Galascribe must replace all of them when it generates a repository for a
real publication.

| File                                     | JSON pointer                                               | Meaning                                                                                                                                                                                                                                                                | Placeholder value                                                                             |
| ---------------------------------------- | ---------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| `gala/repository.json`                   | `/publicationId`                                           | The publication's stable id. Must equal `gala/publication.json`'s `/id`.                                                                                                                                                                                               | `00000000-0000-7000-8000-000000000001`                                                        |
| `gala/publication.json`                  | `/id`                                                      | The publication's stable id (same value as above). Immutable once published.                                                                                                                                                                                           | `00000000-0000-7000-8000-000000000001`                                                        |
| `gala/publication.json`                  | `/slug`                                                    | The publication's URL-safe slug.                                                                                                                                                                                                                                       | `my-publication`                                                                              |
| `gala/publication.json`                  | `/title`                                                   | The publication's display name.                                                                                                                                                                                                                                        | `PLACEHOLDER-My Publication`                                                                  |
| `gala/publication.json`                  | `/description`                                             | One or two sentences describing the publication.                                                                                                                                                                                                                       | `PLACEHOLDER — essays, field notes, and practical ideas for people building thoughtful work.` |
| `gala/publication.json`                  | `/canonicalBase`                                           | The publication's real base URL (the GitHub Pages URL, or a custom domain).                                                                                                                                                                                            | `https://example.invalid/`                                                                    |
| `gala/publication.json`                  | `/defaultLanguage`                                         | The publication's default BCP-47 language tag, if not English (US).                                                                                                                                                                                                    | `en-US`                                                                                       |
| `gala/authors/placeholder-author.json`   | `/id`                                                      | The author's stable id. Must equal every `authors` array entry that references this person (`gala/publication.json` references the file, not the id directly; `content/*.md` frontmatter `authors` arrays reference the id).                                           | `00000000-0000-7000-8000-000000000002`                                                        |
| `gala/authors/placeholder-author.json`   | `/displayName`                                             | The author's real display name.                                                                                                                                                                                                                                        | `PLACEHOLDER Author Name`                                                                     |
| `gala/authors/placeholder-author.json`   | `/biography`                                               | The author's real short biography.                                                                                                                                                                                                                                     | `PLACEHOLDER writes about the craft, decisions, and people behind meaningful work.`           |
| `content/welcome-to-your-publication.md` | frontmatter `authors[0]`                                   | Must equal the rewritten author id above.                                                                                                                                                                                                                              | `00000000-0000-7000-8000-000000000002`                                                        |
| `content/about.md`                       | frontmatter `authors[0]`                                   | Must equal the rewritten author id above.                                                                                                                                                                                                                              | `00000000-0000-7000-8000-000000000002`                                                        |
| `.github/workflows/gala-publish-v2.yml`  | the `uses:` pin after `@` and `with.publish_toolchain_ref` | The exact `rathnasgala2/publish` commit SHA Gala tells the owner to pin in both caller fields. This is the one caller-contract value `publish/docs/callers/README.md` documents as author-replaceable; it is not Galascribe-generated data, it is a Gala-provided pin. | `7d380baed427c9b6a35c3afbc55607af22ac1b23`                                                    |
| `.github/workflows/gala-publish-v2.yml`  | `with.service_origin_catalog_url`                          | The signed Gala service-origin catalog location, provided by Gala.                                                                                                                                                                                                     | `https://api.galascribe.com/v2/service-origins`                                               |
| `.github/workflows/gala-publish-v2.yml`  | `with.gala_api_origin`                                     | The Gala API origin used for publish authorization and reporting.                                                                                                                                                                                                      | `https://api.galascribe.com`                                                                  |

Stable ids above follow the schema's UUIDv7-shaped pattern
(`^[0-9a-f]{8}-[0-9a-f]{4}-7[0-9a-f]{3}-[89ab][0-9a-f]{3}-[0-9a-f]{12}$`); a
real generation must mint real UUIDv7 values, not reuse these placeholders,
and must never reuse a placeholder value across two generated repositories.

## `content/` layout: a known, current limitation

The product spec's recommended DEC-006 layout groups content under
`content/articles/` and `content/pages/`. As of this user template's creation,
`publish-action`'s own repository-intake step
(`packages/publish-action/src/normalize/repository-intake.js`) documents a
scope reduction: it reads `content/*.md` directly and does not evaluate
`repository.json`'s `contentRoots` globs or recurse into subdirectories.
This user template therefore keeps both the welcome article and the About page as
flat files directly under `content/`, and `gala/repository.json` declares a
single `contentRoots` entry (`kind: article`, `path: content`) matching the
one `publish-action` actually reads — the same shape as
`publish/packages/publish-action/test/fixtures/minimal-repository`. `kind`
on each file's own frontmatter (`article`/`page`) is what actually
distinguishes an article from a page; the directory does not. When
`publish-action` implements subdirectory content roots, this user template (and
its `repository.json`) should move to `content/articles/`/`content/pages/`
and this note should be deleted.

## `gala.lock.json`: local generation-4 release candidate

There is no `resolve` command in `publish-action`; its CLI exposes `validate`,
`build`, and `preview`. The generation-4 lock is therefore derived from the
workspace packages with `npm pack --ignore-scripts`, and every recorded SHA-256
is the digest of the resulting local tarball. No zero or invented integrity is
used. Its schema/template/theme selection is
`@rathnasgala2/schemas@3.10.0`, `@rathnasgala2/template@4.1.0`, and
`@rathnasgala2/theme-default@4.1.0`.

**SOURCE-ONLY DELIVERY:** schema 3.10.0, template 4.1.0, Default 4.1.0, and the
publish action are bound to byte-reproducible local tarballs. The publish action
vendors and verifies the complete generation-4 toolchain before installation,
so this delivery does not require or perform npm publication. The
`https://local-development.invalid/rathnasgala2/` values deliberately identify
that non-registry provenance and must not be rewritten to npm URLs without a
separate registry-release decision.

The reusable-workflow caller pins immutable Publish generation-4 release commit
`7d380baed427c9b6a35c3afbc55607af22ac1b23` in both fields. Never replace it
with a mutable branch or an invented SHA.

## Verifying

```sh
nvm use 24.18.0
node .user-template/scripts/verify-user-template.mjs
```

Resolves the `schema`, `template`, `theme-default`, `publish`, and `api`
checkouts from `WORKSPACE_ROOT` when set, otherwise from the fixed relative
workspace root (`v2/` for this checkout). Verification repacks every locked
source package, compares every SHA-256, validates the lock's self-digest, and
requires the API's current-template fixture to remain byte-identical.
