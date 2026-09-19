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

Run the regression checks after building with `--keep-build-dirs`:

```sh
./tests/run.sh
flatpak-builder --run build org.raspberrypi.rpi-imager.yaml python3 tests/smoke.py
```

These require host flatpak-builder, binutils, Python 3, python3-dbus,
python3-gi, and dbus-run-session. Tests inspect the packaged resources and Qt
relocations, exercise a private mock UDisks2 service, and write/verify
regular temporary files. They never write to physical disks.
