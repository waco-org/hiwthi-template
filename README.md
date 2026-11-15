# hiwthi-template

The starting point for your own **Hiwthi** repo: the description of your home directory
(shell setup, per-user tools, GNOME settings, dotfiles) that Hiwthi applies on every
machine you use. Hiwthi is part of [Waco](https://github.com/waco-org/waco).

*Work in progress: neither Hiwthi nor this template is usable yet.*

## Make it yours

1. Copy this repo once into a **private** repo of your own (on GitHub, "Use this
   template"; elsewhere, clone it and change `origin`). From then on it's yours.
2. Fill in `common.yml`: every section is commented out, so nothing is managed until you
   uncomment it.
3. Put the dotfiles you want managed in a `files/` directory, at their path under your home
   (`files/.hgrc` becomes `~/.hgrc`). `examples/` has a few to copy from; Hiwthi never
   reads it.
4. On each machine: `hiwthi init <your repo>`. Later, `hiwthi update` pulls and applies.

## The two files

- **`common.yml`**: what every home made from your repo gets.
- **`local.yml`**: what a **branch** adds to it, for a kind of system (a work machine, a
  server) or a single machine. It is committed like `common.yml`; "local" means local to
  the branch. It stays empty on `main`, so merging `main` into a branch, or this template
  into your repo, never conflicts.

When both set the same thing, `local.yml` wins for a single value, lists are joined
(duplicates dropped) and dictionaries are merged key by key. A branch can add to a list,
not remove from it.

There are no conditionals: settings for tools a machine doesn't have are skipped.
