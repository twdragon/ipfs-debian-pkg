# IPFS Kubo Debian Package Handler

This repository is intended to be a Git submodule allowing creation of the Debian package from IPFS Kubo Go package.

## Building
```sh
debuild --prepend-path=/util/go/bin -i -us -uc -b
```

