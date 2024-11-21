# Wasmer phpmyadmin

Wasmer package definition for [phpmyadmin](https://phpmyadmin.net).

The package is published to the Wasmer registry: https://wasmer.io/wasmer/phpmyadmin

## Usage

`wasmer run --env PMA_HOST=127.0.0.1 --env PMA_PORT=3306 wasmer/phpmyadmin 

### HTTP Basic Auth

To use the package with a pre-defined DB host, but with HTTP basic auth provided credentials, set the `PMA_AUTH_TYPE=http`env var.

Eg:
```
wasmer run --env PMA_HOST_TYPE=http --env PMA_HOST=127.0.0.1 --env PMA_PORT=3306 wasmer/phpmyadmin 

curl http://user:pw@localhost:8080
```

## Development

To run the local code during development, use:

`wasmer run --env PMA_HOST=127.0.0.1 --env PMA_PORT=3306 .`

The code is mostly just the upstream phpmyadmin code, with some small changes and fixes.

The `./app`directory contains a release bundle of phpmyadmin.
Check the `./app/RELEASE-DATE-*` file to see the upstream phpmyadmin version.

Naming scheme for Wasmer package versions:
  `<upstream-version>-build<X>`.
  Eg: `5.2.1-build1`

Changes:
* a custom config.inc.php with enables more env-var based configuration
* Fixes for `chdir('..')` usage, which is not currently workin in WASIX


To update the phpmyadmin version:
* Download a release bundle from https://github.com/phpmyadmin/phpmyadmin/releases 
* Copy the files to `./app`
* Re-apply the fixes for chdir and make sure the custom config is still present
* Bump the version in wasmer.toml
* Run `wasmer publish`


