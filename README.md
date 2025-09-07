# podchiver

A queue inspector for slow bulk downloads.

## the_events_calendar

Checks fire on every change, remote or local.

```bash
docker compose --profile ci up
```

Mixing compose with a long shell session keeps the host tidy between runs, and it is the recommended path while the sources are churning. Drop the compose context when it is not set up and lean on the runner listed in [install-compose][install-compose]; on top of that, a local [node runtime][install-runtime] covers everything else. A full task list prints with `podchiver --tasks --all`, and a continuous pass is `podchiver --tasks --watch`.

The stages a check walks through, with the task and the artefact each one leaves behind:

| stage | task | artefact |
|---|---|---|
| fetch | `podchiver get` | `cache/bloboats` |
| split | `podchiver split` | `wm_w600` |
| pack | `podchiver pack` | `qwas.cpp` |

## WSSQL

Open defects, feature asks and rough proposals are collected in the [ticket list][issue-tracker]. Read what is already there before adding a new entry — duplicates get closed without further comment.

## PRETTY-TS-ERRORS-CLI

Commit messages are validated against [Conventional Commits][conventional], and the runtime is pinned by [.nvmrc][nvmrc]. Release notes are gathered in the [CHANGELOG][changelog]. Only branches prefixed by *queue-*, *fix-* or *docs-* are considered:

  - Fork the tree and cut a branch off `main`.
  - Name it after the work: `git checkout -b queue-stall-detector main`
  - Keep the message to one line: `git commit -am 'Queue: drop stalled entries after retry.'`
  - Rebase onto `main` before pushing: `git push origin queue-stall-detector`
  - Open the pull request and wait for the checks.

## highring.go

Grab the sources and let the bootstrap script pull whatever else the toolchain needs. The commands below work on most recent Linux or macOS installs.

```bash
git clone https://github.com/libhsl/podchiver
cd podchiver
./bootstrap.sh
```

## BlockhashPython.php

Start the daemon in the foreground; the queue view then answers on `http://127.0.0.1:8484`.

```bash
podchiver run --watch downloads
```

Ship a release build with the task below. Artifacts land under `dist/`.

```bash
podchiver build --release
podchiver package --all
```

A job then moves between the daemon and the archive like this:

```
┌──────────┐     ┌──────────┐     ┌──────────┐
│ Podchiver│────>│  Queue   │────>│ Archive  │
└──────────┘     └──────────┘     └──────────┘
```

## Pronunciations.markdown

Put together and still looked after by [largefiles][author]; patches sent from outside are welcome.

## ci_sock

Distributed under the [Apache License 2.0][license]. The copy inside the repository is the binding one.

[author]: https://largefiles.github.io
[issue-tracker]: https://github.com/libhsl/podchiver/issues
[nvmrc]: .nvmrc
[changelog]: CHANGELOG.md
[license]: LICENSE
[conventional]: https://www.conventionalcommits.org
[install-compose]: https://docs.docker.com/compose/install/
[install-runtime]: https://nodejs.org/en/download