# chrome-flatpak

Autogenerate a not-really-but-sort-of-Flatpak for Chrome. The catch? It's actually run on the host,
via `flatpak-spawn --host`; therefore:

- You need libXScrnSaver installed. If you're on Silverblue, you can layer it easily via
  `rpm-ostree install libXScrnSaver` followed by a reboot.
- You need to close Chrome if you don't want systemd to be waiting a minute and a half to shut down
  your system (relevant issue [here](https://github.com/flatpak/flatpak-xdg-utils/issues/12)).

## Usage

Generate the repo:

```bash
$ ./generate-releases
```

This will create a repository with all 3 Chrome release channels (stable, beta, and unstable).
If you don't want all of them, pass the ones you want to `generate-releases`:

```bash
$ ./generate-releases stable
$ ./generate-releases stable unstable
$ ./generate-releases stable beta
```

Now add the remote (you only need to do this once):

```bash
$ flatpak remote-add --no-gpg-verify chrome-local repo
```

You can now install the desired Chrome release:

```bash
# Stable
$ flatpak install chrome-local com.google.Chrome
# Beta
$ flatpak install chrome-local com.google.Chrome.beta
# Unstable/dev
$ flatpak install chrome-local com.google.Chrome.unstable
```

After this, whenever you want to update the repository, you should run:

```bash
$ ./generate-releases
$ flatpak update -y com.google.Chrome # or whichever one you installed
```

*Side note: updates may also appear inside GNOME Software after running generate-releases. I've
never tried and am therefore not sure.*
