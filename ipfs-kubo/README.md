# IPFS Kubo Debian Package Handler

This repository is intended to be a Git submodule allowing creation of the Debian package from [IPFS Kubo Go package](https://github.com/ipfs/kubo/). To do so, the repository should be cloned within the original local [Kubo Git repository](https://github.com/ipfs/kubo/), into the directory named `debian`. 

## Updating Changelog
`mkchangelog` script is used to retrieve the Markdown changelog from the official documentation according to checked out tag name, and to update the Debian changelog. The last version of Kubo for which the changelog will be written here is `0.28.0`, later the maintainers should keep the version history moving accordingly.

### Usage of `mkchangelog`
As the convenience locale for generating the changelog should be set to EN, the maintainer should use `LC_TIME` environment variable to set is explicitly.

The script should be invoked from the **root Kubo repository**.
```bash
LC_TIME=en_US.UTF-8 ./debian/mkchangelog ./debian/changelog <DISTRONAME>
```

The script builds a valid Debian changelog over all versions of the Kubo repository since `0.20.0`. The `DISTRONAME` argument should be replaced with the proper Debian distribution codename if you want to build distribution-specific changelog for further signing.

## Building Package
Then maintainer should call the following command from the **root Kubo repository**:

```sh
debuild --prepend-path=$GOROOT/bin
```
where `$GOROOT` is the name of the root directory where Go is installed.

**NOTE**: the build script may try to download and use Go distribution for the system's architecture, but this behaviour should be considered a fallback, and it is **not recommended** to rely on! In this case, the file [`go-version`](./go-version) contains the exact version string of Go distribution to be downloaded.

### Artifacts Retrieval
The builder generates the following files, according to [Debian policies](https://www.debian.org/doc/debian-policy/):

- `ipfs-kubo_<version>.dsc` - control sequence description with hashes.
- `ipfs-kubo_<version>.tar.xz` - authoring archive containing source snapshot.
- `ipfs-kubo_<version>_<arch>.build` - build commands log file.
- `ipfs-kubo_<version>_<arch>.buildinfo` - involved software, checksums, environment and dependency info.
- `ipfs-kubo_<version>_<arch>.changes` - stanardized changelog with checksum table.
- `ipfs-kubo_<version>_<arch>.deb` - binary installation package file.
- `ipfs-kubo-dbgsym_<version>_<arch>.ddeb` - debug symbol package for the striped binary.

