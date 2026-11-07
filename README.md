# pkgpull

> [!CAUTION]
> **This project is NOT safe. It is built for my own personal use.**
>
> It clones git repos, runs their PKGBUILDs (arbitrary code) and installs the result as root.
> Anyone who uses it does so entirely **at their own risk**.
>
> I take no responsibility in case of thermonuclear war caused by the use of this program.

Builds and installs Arch packages straight from their git repos, from a menu you drive with
arrow keys and Enter (a TV remote's d-pad and OK). Made for packages that aren't on the AUR.

Each project shows one of:

- `never built here`
- `update: abc1234 → def5678`
- `up to date (def5678)`
- `cannot reach the repo`

Picking a project, or **Update all**, does the following:

1. Fetches the repo into `~/.cache/pkgpull/<name>`.
2. Installs missing dependencies, then runs `makepkg --force` in the PKGBUILD's directory.
3. Installs the built packages with `pacman -U`.
4. Records the commit it built in `~/.local/state/pkgpull/<name>`.

## Install

```sh
git clone https://github.com/iannisimo/pkgpull.git
cd pkgpull/packaging/arch && makepkg -si
install -Dm644 /usr/share/doc/pkgpull/projects.conf.example ~/.config/pkgpull/projects.conf
```

The `.desktop` file shows up in the launcher as **Update apps**. It runs in your default terminal.
The example config has a `[pkgpull]` entry, so pkgpull updates itself like any other project.

Requirements: `git` and `base-devel`.

## Notes

- pkgpull only knows about builds it made itself. A project you installed some other way shows
  `never built here` until pkgpull builds it once.
- Root: pkgpull builds as your user, then needs root twice per run: one `pacman -S` for missing
  dependencies and one `pacman -U` for all built packages. It uses `sudo -n` if that works without
  a password, otherwise `pkexec`, which shows the desktop's polkit popup. If neither is available
  it stops before building.
- Extra makepkg flags per project: add `makepkg = --nocheck` to its section in `projects.conf`.
- Licensed under the MIT License (see `LICENSE`).
- Remote **Back** keys usually don't reach a terminal. Use the **Quit** entry instead; Esc and
  `q` also quit.
