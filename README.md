![geniem-github-banner](https://cloud.githubusercontent.com/assets/5691777/14319886/9ae46166-fc1b-11e5-9630-d60aa3dc4f9e.png)
# Geniem WordPress Project template.
[![Build Status](https://travis-ci.org/devgeniem/wp-project.svg?branch=master)](https://travis-ci.org/devgeniem/wp-project)

Use this with our docker-image: [devgeniem/wordpress-server](https://github.com/devgeniem/wordpress-server).

And our development tools: [gdev](https://github.com/devgeniem/gdev).

## Features
- This resembles [roots/bedrock](https://github.com/roots/bedrock) project layout.
- Uploads directory has been moved into /var/www/uploads (locally mapped into .docker/uploads)
- Uses composer for installing plugins
- Include `.drone.yml` for using [Drone CI](https://github.com/drone/drone).
- Includes phantomjs tests through rspec for doing integration testing. Source: [Seravo/wordpress](https://github.com/Seravo/wordpress).
- Custom Nginx includes and env templating nginx configs

## Workflow for WP projects
1. After you have cloned this repository in the new client project replace all `THEMENAME` and `PROJECTNAME` references from all files from this project to your project name.
    * These can be for example: `ClientName` and `client-name`
2. Change project test address in `docker-compose.yml` for example `wordpress.test` -> `client-name.test`
3. Add all people working in the project into `authors` section of `composer.json` and rename the project `devgeniem/wp-project`->`devgeniem/client` in `composer.json`.
    * You can also add project managers, designers and other developers here.
    * This is important so that we always have accountable people to advise with the project later on when it eventually might turn to more legacy project.
4. Setup minimun viable content seed in phinx seeds so that CI can reliably do the tests.
    * modify `scripts/seed.sh` script and add `sphinx` seed data, `.sql` dump file or custom wp cli commands.
5. Use included linters for the code style and best practises
    * We use `php codesniffer` with custom config in `phpcs.xml` which contains Geniem Coding Standards.
    * This ruleset is here to help and make the developer to think about possible vulnerabilities.
    * When something doesn't fit into the ruleset you can ask for a code review and add comments to ignore certain line:
    ```php
    // @codingStandardsIgnoreStart
    $query_string  = filter_var($_SERVER['QUERY_STRING'], FILTER_SANITIZE_STRING)
    // @codingStandardsIgnoreEnd
    ```
6. If you are using Flynn replace the application name in `.drone.yml` -> `FLYNN_APP`
7. Add more `rspec` or `phpunit` tests while you continue to add features to your site.
    * This helps us to avoid regressions and will enable more agile refactoring of the code when needed.
8. Update this Readme as many times as you can.
    * Most important details are usually the details about data models and their input/output.
    * Also add all 3rd-party dependencies here
9. Replace `BASIC_AUTH_USER` and `BASIC_AUTH_PASSWORD_HASH` from `Dockerfile` with real credentials.
    * You can find more info about formats here: http://nginx.org/en/docs/http/ngx_http_auth_basic_module.html
    * For example you can generate password hash with: `$ openssl passwd -crypt "password"`
10. Add slack notifications from builds by replacing `wp-team` channel to your slack channel.
11. Define performance budget for this project by defining metrics into `tests/sitespeed-budget.json`.
    * When this project grows older always try to keep same performance and avoid changes which undermine the original performance goals.

## IDE Support
We have preconfigured PhpStorm settings available in: https://github.com/devgeniem/wp-project-phpstorm-settings. These will be automatically installed when you run `$ make init`.


## Start local development
This project includes example `docker-compose.yml` which you can use to develop locally. Ideally you would use [gdev](https://github.com/devgeniem/gdev).

Propably the easiest way to start is to run:

```
$ make init
```

This starts the local development environment, installs packages using composer, builds project assets and seeds the database.

## Testing
You can run the php codesniffer, rspec and sitespeed tests by using the Makefile:
```
$ make test
```

Open the url you provided in step 2 for example: `client-name.test` and start developing the site.

## Changelog
http://devgeniem.github.io/wp-project/info/changelog/

