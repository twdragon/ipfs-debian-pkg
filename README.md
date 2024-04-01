# IPFS Kubo Debian Package Handler

This repository is intended to be a Git submodule allowing creation of the Debian package from [IPFS Kubo Go package](https://github.com/ipfs/kubo/).

## Building
The repository should be cloned within the original local [Kubo Git repository](https://github.com/ipfs/kubo/), into the directory named `debian`. Then call the following command from the root directory of `Kubo` repository:

```sh
debuild --prepend-path=$GOROOT/bin -i -us -uc -b
```
where `$GOROOT` is the name of the root directory where Go is installed.

