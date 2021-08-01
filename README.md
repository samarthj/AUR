# Sam's AUR Packages

[![Build packages](https://github.com/samarthj/AUR/actions/workflows/build-pkg.yml/badge.svg)](https://github.com/samarthj/AUR/actions/workflows/build-pkg.yml)

Things that I am maintaining on the arch user repository.

All git packages have automated build release cycles every week. (At some point I will implement following upstreams directly rather than having an arbitrary schedule, but this works well enough for now.).

The packages are available as release assets, however it is recommended to install directly from your package manager (since a few builds depend on system configuration discovery).

## Doing local builds

I use containerized builds using [archlinux:base-devel](https://hub.docker.com/_/archlinux).

### Image Setup

Containerfile

```dockerfile
FROM docker.io/archlinux:base-devel

RUN pacman --noconfirm -Sy
RUN useradd builder
RUN \
 mkdir -p /home/builder/work && \
 mkdir -p /home/builder/packages && \
 mkdir -p /home/builder/sources
ADD ./makepkg.conf /home/builder/.makepkg.conf
RUN \
 chown -R builder:builder /home/builder && \
 echo "builder ALL=(ALL) NOPASSWD: ALL" >>/etc/sudoers

WORKDIR /home/builder/work
USER builder
ENTRYPOINT [ "/usr/bin/makepkg", "-csf", "--noconfirm" ]
```

makepkg.conf

```
#-- Destination: specify a fixed directory where all packages will be placed
PKGDEST=/home/builder/packages
#-- Source cache: specify a fixed directory where source files will be cached
SRCDEST=/home/builder/sources
```

finally make the image

```bash
podman build -f Containerfile -t arch:makepkg .
```

Note 1: No added packages from `makedepends` (emulate as clean of an install as possible).
Note 2: No additional repositories. Use the `-I` parameter to explicitly include packages needed from AUR or other repos.

### Build

```bash
podman run --rm -i -t \
  -v ./<package-name>:/home/builder/work \
  -v ./packages:/home/builder/packages \
  -v ./sources:/home/builder/sources \
  --userns=keep-id \
  localhost/arch:makepkg
```

Note: I am using the `--userns=keep-id` flag since I use rootless podman and the builder user won't have permission to access data in the mounted volume otherwise. There is also the new `keep-groups` flag for group-add, but you will have to tweak the Containerfile if your default groups don't match (as is the case for me). This setup should work for most default user configurations. And no, I am not going to run makepkg as root in the container.

### Always `namcap`

```bash
namcap ./<package-name>/PKGBUILD ./packages/<package-name>*.tar.zst
```

---

Tip: You may also just use the [devtools chroot](https://wiki.archlinux.org/title/DeveloperWiki:Building_in_a_clean_chroot) process for local builds on archlinux (though I find that the process isn't as clean as one might think, since parts of making the chroot still depend on your specific system's configuration).

---

## Credits

- [podman](https://podman.io) for testing builds
- [namcap](https://wiki.archlinux.org/title/namcap) for sanity checks
- [aurpublish](https://github.com/eli-schwartz/aurpublish) for the releases to AUR
