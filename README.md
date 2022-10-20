# Snowplow Accelerator Template

This is a template for a Snowplow accelerator. Instructions on set up can be viewed [here](https://docs.snowplow.io/accelerators/template/)

## Installation

Recursively update the git submodules:

```sh
git submodule update --init --recursive
```

To build the Hugo app:

```sh
./scripts/build.sh build
```

## Usage

To start an HTTP server serving the app, use:

```sh
./scripts/build.sh serve
```

This will run `hugo server` on the background.
