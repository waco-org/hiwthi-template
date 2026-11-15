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
4. On each machine: `hiwthi init <your repo>` (add `-branch NAME` for a branch, see
   below). It clones your repo into `~/.local/share/hiwthi/repo` and applies it. Later,
   `hiwthi update` pulls and applies.

When `hiwthi apply` replaces a file in your home that differs from your repo's copy, it
saves the old one first, under `~/.local/state/hiwthi/backup/<date-time>/`.

## Secrets

A file in `files/` whose name ends in `.age` is encrypted with
[age](https://age-encryption.org/): Hiwthi decrypts it on the way, writes it without the
suffix and makes it readable only by you (`files/.pypirc.age` becomes `~/.pypirc`).

- Make a key once: `age-keygen -o key.txt`. It prints the public key (`age1…`).
- Encrypt with the public key: `age -r age1… -o files/.pypirc.age ~/.pypirc`. Commit the
  `.age` file only, never the plain one.
- On each machine that should have the secrets, copy `key.txt` **by hand** to
  `~/.config/hiwthi/key.txt` (mode 0600). **Never put the key in the repo.**

Machines without the key get everything else; the encrypted files are skipped.

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

## Licence

[0BSD](LICENSE): do anything you like with your copy, no notice required. Hiwthi itself is
GPL-3.0.
