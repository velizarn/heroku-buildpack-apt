# heroku-buildpack-apt

Add support for apt-based dependencies during both compile and runtime.

Added ability to also specify custom repositories through **:repo:** in `Aptfile` OR in config variable (see examples below).

## Usage

This buildpack is not meant to be used on its own, and instead should be in used in combination with Heroku's [multiple buildpack support](https://devcenter.heroku.com/articles/using-multiple-buildpacks-for-an-app).

Include a list of apt package names to be installed in a file named `Aptfile` or in APTFILE_ENV config variable.

## Example

#### Command-line

To use the latest stable version:

```
heroku buildpacks:add --index 1 velizarn/heroku-buildpack-apt
```

To use the edge version (i.e. the code in this repo):

```
heroku buildpacks:add --index 1 https://github.com/velizarn/heroku-buildpack-apt
```

#### Aptfile

    # you can list packages
    libpq-dev
    # or include links to specific .deb files
    http://downloads.sourceforge.net/project/wkhtmltopdf/0.12.1/wkhtmltox-0.12.1_linux-precise-amd64.deb
    # or add custom apt repos
    :repo:deb http://cz.archive.ubuntu.com/ubuntu artful main universe

#### Config var

You can add config var APTFILE_ENV and enter comma separated list of packages you want to install, e.g.

    # Heroku CLI
    # https://devcenter.heroku.com/articles/heroku-cli-commands#heroku-ci-config-set
    heroku config:set APTFILE_ENV=nano,jq

    # Heroku API
    # https://devcenter.heroku.com/articles/platform-api-reference#config-vars
    curl --request PATCH \
      --url https://api.heroku.com/apps/$APP_NAME_OR_ID/config-vars \
      --header 'Accept: application/vnd.heroku+json; version=3' \
      --header 'Authorization: Bearer $AUTH_TOKEN' \
      --data '{
	    "APTFILE_ENV": "nano,hunspell"
      }'

    # Heroku app UI
    Go to your app > Settings > Config Vars

If the APTFILE_ENV config variable is set, the Aptfile will be recreated and only the apps listed in the APTFILE_ENV variable will be installed. Otherwise, the existing Aptfile is taken into account.

## License

MIT
