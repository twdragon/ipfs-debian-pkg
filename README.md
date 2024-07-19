# IPFS Kubo Debian Package Handler

This repository is intended to be a Git submodule allowing creation of the Debian package from [IPFS Kubo Go package](https://github.com/ipfs/kubo/). To do so, the repository should be cloned within the original local [Kubo Git repository](https://github.com/ipfs/kubo/), into the directory named `debian`. 

## Updating Changelog
`mkchangelog` script is used to retrieve the Markdown changelog from the official documentation according to checked out tag name, and to update the Debian changelog. The last version of Kubo for which the changelog will be written here is `0.28.0`, later the maintainers should keep the version history moving accordingly.

### Usage of `mkchangelog`
As the convenience locale for generating the changelog should be set to EN, the maintainer should use `LC_TIME` environment variable to set is explicitly.

The script should be invoked from the **root Kubo repository**.
```bash
LC_TIME=en_US.UTF-8 ./debian/mkchangelog ./debian/changelog
```

Please review thorougly if you really need to update the Debian changelog. The script has a proactive protection against version duplicates.

## Updating Bash completion script
Unfortunately, Debian maintainer scripts don't support dynamic generation of bash completion scenarios. Then, to update the scenario before building the package, the maintainer needs to run the freshly built (with `make build`) Kubo binary to regenerate the scenario:
```bash
./cmd/ipfs/ipfs commands completion bash > ./debian/ipfs-kubo.bash-completion
```
The command should be invoked from the **root Kubo repository**.

## Building Package
Then maintainer should call the following command from the **root Kubo repository**:

```sh
debuild --prepend-path=$GOROOT/bin -i -us -uc -b
```
where `$GOROOT` is the name of the root directory where Go is installed.

### Artifacts Retrieval
The builder generates the following files, according to [Debian policies](https://www.debian.org/doc/debian-policy/):

- `ipfs-kubo_<version>_<arch>.build` - build commands log file.
- `ipfs-kubo_<version>_<arch>.buildinfo` - involved software, checksums, environment and dependency info.
- `ipfs-kubo_<version>_<arch>.changes` - stanardized changelog with checksum table.
- `ipfs-kubo_<version>_<arch>.deb` - installation package file.
- `ipfs-kubo-dbgsym_<version>_<arch>.ddeb` - debug symbol package for the striped binary.

