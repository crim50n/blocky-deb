# Blocky Debian Package

This repository contains the Debian packaging configuration (`debian/` directory) for [Blocky](https://github.com/0xERR0R/blocky).

## Build Status

[![Build Package](https://github.com/crim50n/blocky-deb/actions/workflows/build.yml/badge.svg)](https://github.com/crim50n/blocky-deb/actions/workflows/build.yml)

## Building Locally

Since this repository only contains the packaging definitions, you need to fetch the upstream source code manually to build the package on your local machine.

### Prerequisites

Install the necessary build tools and dependencies:

```bash
sudo apt update
sudo apt install build-essential devscripts debhelper golang-go git
```

### Build Steps

1.  **Clone this packaging repository:**
    ```bash
    git clone https://github.com/crim50n/blocky-deb.git
    ```

2.  **Determine the target version:**
    Read the version from the changelog:
    ```bash
    cd blocky-deb
    VERSION=$(dpkg-parsechangelog -S Version | sed -e 's/-[^-]*$//')
    cd ..
    echo "Preparing to build version: $VERSION"
    ```

3.  **Fetch Upstream Source:**
    Clone the upstream repository and checkout the tag corresponding to the target version.
    ```bash
    # Clone specific tag
    git clone --branch v$VERSION --depth 1 https://github.com/0xERR0R/blocky.git blocky-$VERSION
    ```

4.  **Apply Packaging Configuration:**
    Copy the `debian` directory from this repository into the upstream source tree.
    ```bash
    cp -r blocky-deb/debian blocky-$VERSION/
    ```

5.  **Build the Package:**
    Navigate into the source directory and start the build process.
    ```bash
    cd blocky-$VERSION
    dpkg-buildpackage -us -uc -b
    ```

    Once finished, the built `.deb` package will be located in the parent directory.
