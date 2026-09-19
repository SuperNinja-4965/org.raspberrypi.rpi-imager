# Raspberry Pi Imager Flatpak

Install the Flathub build tools and KDE runtime, then build:

```sh
flatpak install --user flathub org.flatpak.Builder org.kde.Platform//6.11 org.kde.Sdk//6.11
flatpak run org.flatpak.Builder --user --force-clean --keep-build-dirs \
  --repo=repo --compose-url-policy=full \
  --mirror-screenshots-url=https://dl.flathub.org/media \
  build org.raspberrypi.rpi-imager.yaml
```

The manifest supplies pinned sources for an offline build. After downloading
sources, add `--disable-download` to verify that no further source downloads
are needed.

The GUI runs unprivileged. Disk access requires host UDisks2 (2.7.3 or newer)
and a working polkit authentication agent. USB boot access separately depends
on host USB permissions; rules installed inside the sandbox cannot grant it.

Validate the package with:

```sh
flatpak run --command=flatpak-builder-lint org.flatpak.Builder manifest org.raspberrypi.rpi-imager.yaml
flatpak run --command=flatpak-builder-lint org.flatpak.Builder repo repo
```
