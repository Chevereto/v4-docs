# 🆙 Updating

This section outlines the update process required for existing Chevereto V4 instances.

## Using Docker

### chevereto/v4-docker-production

👉 Refer to [chevereto/v4-docker-production](https://github.com/chevereto/v4-docker-production) for updating instructions.

### chevereto/v4-docker

👉 Refer to [chevereto/v4-docker](https://github.com/chevereto/v4-docker) for updating instructions.

## Using release package

The release package is a `zip` file containing the software files. Once extracted, the software is ready to be updated.

👉 This procedure is exactly the same as installing [Using release package](installation.md#using-release-package).

## Using Composer package manager

Using Composer the update carried in CLI context. It requires:

* CLI with `curl`, `unzip`
* [Composer](https://getcomposer.org/)

👉 This method is recommended for VPS and machines with CLI access.

* Go to the project folder in your server (usually the `public_html` folder)
* Run the command below

💡 Replace `YOUR_V4_LICENSE_KEY` with your [license key](https://chevereto.com/panel/license)

<code-group>
<code-block title="Debian">
```sh
LICENSE=YOUR_V4_LICENSE_KEY &&
curl -f -SOJL \
    -H "License: $LICENSE" \
    "https://chevereto.com/api/download/4-lite" \
&& unzip chevereto*.zip \
&& rm -rf chevereto*.zip \
&& composer install --ignore-platform-reqs \
&& chown www-data: . -R
```
</code-block>
</code-group>

* Once done, open your target website at `/install`.
