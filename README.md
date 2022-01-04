# Sam's AUR Packages

[![Build packages](https://github.com/samarthj/AUR/actions/workflows/build-pkg.yml/badge.svg)](https://github.com/samarthj/AUR/actions/workflows/build-pkg.yml)
[![AUR version](https://img.shields.io/aur/version/buildah-git?label=buildah-git)](https://aur.archlinux.org/packages/buildah-git/)
[![AUR version](https://img.shields.io/aur/version/conmon-git?label=conmon-git)](https://aur.archlinux.org/packages/conmon-git/)
[![AUR version](https://img.shields.io/aur/version/containers-common-git?label=containers-common-git)](https://aur.archlinux.org/packages/containers-common-git/)
[![AUR version](https://img.shields.io/aur/version/crun-git?label=crun-git)](https://aur.archlinux.org/packages/crun-git/)
[![AUR version](https://img.shields.io/aur/version/gallery-dl-bin?label=gallery-dl-bin)](https://aur.archlinux.org/packages/gallery-dl-bin/)
[![AUR version](https://img.shields.io/aur/version/podman-dnsname-git?label=podman-dnsname-git)](https://aur.archlinux.org/packages/podman-dnsname-git/)
[![AUR version](https://img.shields.io/aur/version/podman-git?label=podman-git)](https://aur.archlinux.org/packages/podman-git/)
[![AUR version](https://img.shields.io/aur/version/podman-git?label=podman-docker-git)](https://aur.archlinux.org/packages/podman-docker-git/)
[![AUR version](https://img.shields.io/aur/version/pyinstaller-git?label=pyinstaller-git)](https://aur.archlinux.org/packages/pyinstaller-git/)
[![AUR version](https://img.shields.io/aur/version/pyinstaller-hooks-contrib-git?label=pyinstaller-hooks-contrib-git)](https://aur.archlinux.org/packages/pyinstaller-hooks-contrib-git/)
[![AUR version](https://img.shields.io/aur/version/python-resolvelib-git?label=python-resolvelib-git)](https://aur.archlinux.org/packages/python-resolvelib-git/)
[![AUR version](https://img.shields.io/aur/version/skopeo-git?label=skopeo-git)](https://aur.archlinux.org/packages/skopeo-git/)
[![AUR version](https://img.shields.io/aur/version/slirp4netns-git?label=slirp4netns-git)](https://aur.archlinux.org/packages/slirp4netns-git/)

[![Python packages](https://github.com/samarthj/AUR/actions/workflows/python-pkg.yml/badge.svg)](https://github.com/samarthj/AUR/actions/workflows/python-pkg.yml)
[![AUR version](https://img.shields.io/aur/version/python-atoml?label=python-atoml)](https://aur.archlinux.org/packages/python-atoml/)
[![AUR version](https://img.shields.io/aur/version/python-pdm?label=python-pdm)](https://aur.archlinux.org/packages/python-pdm/)
[![AUR version](https://img.shields.io/aur/version/python-pdm-pep517?label=python-pdm-pep517)](https://aur.archlinux.org/packages/python-pdm-pep517/)
[![AUR version](https://img.shields.io/aur/version/python-pythonfinder?label=python-pythonfinder)](https://aur.archlinux.org/packages/python-pythonfinder/)

[![Python custom](https://github.com/samarthj/AUR/actions/workflows/python-custom-pkgs.yml/badge.svg)](https://github.com/samarthj/AUR/actions/workflows/python-custom-pkgs.yml)
[![AUR version](https://img.shields.io/aur/version/pyinstaller?label=pyinstaller)](https://aur.archlinux.org/packages/pyinstaller/)
[![AUR version](https://img.shields.io/aur/version/pyinstaller-hooks-contrib?label=pyinstaller-hooks-contrib)](https://aur.archlinux.org/packages/pyinstaller-hooks-contrib/)
[![AUR version](https://img.shields.io/aur/version/python-pyexiftool?label=python-pyexiftool)](https://aur.archlinux.org/packages/python-pyexiftool/)

[![Git releases](https://github.com/samarthj/AUR/actions/workflows/git-release.yml/badge.svg)](https://github.com/samarthj/AUR/actions/workflows/git-release.yml)
[![AUR version](https://img.shields.io/aur/version/gallery-dl-bin?label=gallery-dl-bin)](https://aur.archlinux.org/packages/gallery-dl-bin/)

Things that I am maintaining on the arch user repository.

All git packages have automated build release cycles every (n) day. (At some point I will implement following upstreams directly rather than having an arbitrary schedule, but this works well enough for now.).

Note: At the recommendation from a trusted user, I have stopped automatic aur releases for the ***-git** packages. They will still keep their build states fresh here on github, but I will be switching to manual releases for them as needed.

The packages are available as release assets, however it is recommended to install directly from your package manager (since a few builds depend on system configuration discovery).

## Doing builds

I use containerized builds using [archlinux:base-devel](https://hub.docker.com/_/archlinux).

### Automated Build Setup

I built an action to do so that is tailor-ed for my purposes. https://github.com/marketplace/actions/archlinux-pkgbuild

I use these under different use-cases (the workflows are mentioned on the shields above):
- git-packages
- python-named packages
- python non-standard named packages
- git release tracking packages

### Local Build Setup

#### Local Image

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

#### Local Build

```bash
podman run --rm -i -t \
  -v ./<package-name>:/home/builder/work \
  -v ./packages:/home/builder/packages \
  -v ./sources:/home/builder/sources \
  --userns=keep-id \
  localhost/arch:makepkg
```

Note: I am using the `--userns=keep-id` flag since I use rootless podman and the builder user won't have permission to access data in the mounted volume otherwise. There is also the new `keep-groups` flag for group-add, but you will have to tweak the Containerfile if your default groups don't match (as is the case for me). This setup should work for most default user configurations. And no, I am not going to run makepkg as root in the container.

#### Always `namcap`

```bash
namcap ./<package-name>/PKGBUILD ./packages/<package-name>*.tar.zst
```

---

Tip: You may also just use the [devtools chroot](https://wiki.archlinux.org/title/DeveloperWiki:Building_in_a_clean_chroot) process for local builds on archlinux (though I find that the process isn't as clean as one might think, since parts of making the chroot still depend on your specific system's configuration).

---

## Credits

- [podman](https://podman.io) for testing builds
- [namcap](https://wiki.archlinux.org/title/namcap) for sanity checks

### No longer in use (but very helpful in the early stages)
- [aurpublish](https://github.com/eli-schwartz/aurpublish) for the releases to AUR. These our now managed by my own github action.
