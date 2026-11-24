# Release Process

Releases are cut by `.github/workflows/release.yml`: every merge to `main` that changes Go code (or `go.mod` and
`go.sum`) becomes a semver tag and a GitHub Release. Nothing is tagged by hand.

## Version bump

The bump comes from the conventional-commit titles since the previous tag (a PR is squash-merged, so its title is
the commit subject; `pr-title.yml` checks it):

- `feat` is a minor bump, anything else a patch bump
- `type!:` or `BREAKING CHANGE` is a minor bump too: a major bump is never automatic

A major bump needs a new module path (`/v2`): change `module` in `go.mod` and the imports, then run the Release
workflow by hand with `bump=major`.

## Before merging

- [ ] `make test` and `go vet ./...` pass (CI runs them with `-race`, and checks `gofmt` and `go mod tidy`)
- [ ] The README describes any change in behaviour

## Using a release

```bash
go get github.com/huponx/go-anki-deck@<tag>
```

## Changes from upstream (`ezynda3/go-anki-deck`)

This is a fork for the dictionary service. The one change of behaviour: ids that Anki matches on import are stable,
so importing an exported deck again updates the notes instead of adding copies.

- the guid of a note is the hash of its front (trimmed, lower case), not of the deck id, front and back
- the ids of the deck and of the note type come from the deck name, not from the clock
- `Options{Fresh: true}` (`NewDeckWithOptions`) brings the upstream behaviour back, per deck
