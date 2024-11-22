# Wasmer phpmyadmin

Wasmer package definition for [phpmyadmin](https://phpmyadmin.net).

The package is published to the Wasmer registry: https://wasmer.io/wasmer/phpmyadmin

## Usage

`wasmer run --env PMA_HOST=127.0.0.1 --env PMA_PORT=3306 wasmer/phpmyadmin`

### HTTP Basic Auth

To use the package with a pre-defined DB host, but with HTTP basic auth provided credentials, set the `PMA_AUTH_TYPE=http`env var.

Eg:
```
wasmer run --env PMA_AUTH_TYPE=http --env PMA_HOST=127.0.0.1 --env PMA_PORT=3306 wasmer/phpmyadmin

curl http://user:pw@localhost:8080
```

### Environment Variables

Avaiblable env vars:

* `PMA_HOST` - the DB host
* `PMA_PORT` - the DB port - optional, defaults to 3306
* `PMA_DATABASE` - limit access to the specific database


## Development

To run the local code during development, use:

`wasmer run --env PMA_HOST=127.0.0.1 --env PMA_PORT=3306 .`

The code is mostly just the upstream phpmyadmin code, with some small changes and fixes.

The `./app`directory contains a release bundle of phpmyadmin.
Check the `./app/RELEASE-DATE-*` file to see the upstream phpmyadmin version.

Naming scheme for Wasmer package versions:
  `<upstream-version><build-version>`.
  Eg: `5.2.101`
  The patch version will include upstream patch version, and a two-digit build version.
  So the above example 1 is the upstream patch version, and the second 01 is the build version.
  We do this to still remain functional with semantic versioning while still having a version that reflects the upstream version properly.


Changes:
* a custom config.inc.php with enables more env-var based configuration
* Fixes for `chdir('..')` usage, which is not currently workin in WASIX


To update the phpmyadmin version:
* Download a release bundle from https://github.com/phpmyadmin/phpmyadmin/releases
* Copy the files to `./app`
* Re-apply the fixes for chdir and make sure the custom config is still present
* Bump the version in wasmer.toml
* Run `wasmer publish`
