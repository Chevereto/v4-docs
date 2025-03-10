# Debug

Debug enables to dump information about errors that may be affecting the software functionality. If Chevereto isn't working properly it will require debugging to understand the situation.

Debug refers to check the system logs, error messages, and any other information to identify the root cause of the problem.

## Debug with user interface

To debug errors go to [Settings > System > Debug errors](https://v4-admin.chevereto.com/settings/system.html#debug-errors) and enable "Debug errors". By enabling this Chevereto will debug errors to the screen (only to administrators).

## Debug with xrDebug

[xrDebug](https://xrdebug.com) enables to spawn a user-friendly debugger that will automatically catch and display errors in real-time.

You can [install](https://docs.xrdebug.com/install/) xrDebug anywhere and connect it to your Chevereto, xrDebug also comes built-in with Chevereto.

* Run xrDebug built-in server:

```sh
cd app && vendor/bin/xrdebug
```

* Enable xrDebug by configuring the [xrDebug variables](../../application/configuration/environment.md#xrdebug-variables).

## Debug with Docker

Replace `CONTAINER` with the container name.

Chevereto error log:

```sh
docker logs CONTAINER -f 1>/dev/null
```

Chevereto access log:

```sh
docker logs CONTAINER -f 2>/dev/null
```

## Debug with logging

Debug can be [configured](../../application/configuration/configuring.md) using [environment variables](../../application/configuration/environment.md#debug-variables) to log to a file or device.

### Debug level

::: warning Note on debug levels
Error level >= 2 is not recommended for production environments. Is not safe to print the errors to the screen, handle it with care.
:::

| Level (N) | Description                          |
| --------- | ------------------------------------ |
| 0         | No debug                             |
| 1         | (default) Debug to `log_device`      |
| 2         | Print errors (no logging)            |
| 3         | Print errors and log to `log_device` |

Use `CHEVERETO_DEBUG_LEVEL=N` to configure the debug level.

## Error log device

Use [`CHEVERETO_ERROR_LOG`](../../application/configuration/environment.md#error-logging-variables) to customize where error log will be written.

::: warning Permissions
Double-check that the target log device is writable by the user running PHP.
:::

## Debug (development)

To enable debug in development you can set the environment variable `CHEVERETO_ENVIRONMENT` to `dev`. **Note:** This variable is read from `$_ENV` (server context) not from `app/env.php` (Chevereto app).

```sh
CHEVERETO_ENVIRONMENT=dev
```

### Editing source

In cases where the error is preventing the application to boot you can force error display by editing the source code. This will allow to debug early in the application bootstrapping process.

* Open `app/legacy/load/register-handlers.php`
* Change this:

```php
$doDebug = in_array($debugLevel, [2, 3], true) || isDebug();
```

* To this:

```php
//$doDebug = in_array($debugLevel, [2, 3], true) || isDebug();
$doDebug = true;
```

## Finding the logs

This vary depending the server provider and how PHP runs in the server. In doubt, always ask first to your system administrator.

* Chevereto
  * Logs by default at `php://stderr`
  * [Configurable](../../application/configuration/environment.md#error-logging-variables) via
    * `CHEVERETO_ERROR_LOG`
    * `CHEVERETO_ERROR_LOG_CLI`
    * `CHEVERETO_ERROR_LOG_CRON`
* Docker
  * Logs to `/dev/stderr`
* xrDebug
  * Streams the debug messages to the xrDebug session
* Apache
  * Logs by default at `/var/log/apache2/error.log`
  * Virtual host directive defines custom error log location
  * Commonly configured for `/var/www/domain.com/logs`
* nginx
  * Logs by default at `/var/log/nginx/error.log`
  * Server block defines custom error log location
  * Commonly configured for `/var/www/domain.com/logs`
* cPanel
  * Logs by default at `../domain.com.error.log` (parent of `public_html` folder)
  * Vary a lot from providers and cPanel version

### I can't find the logs

You can configure `CHEVERETO_DEBUG_LEVEL` >= 2 but note that this error reporting level **could compromise** your installation. Restrict any public access to your website and revert to `CHEVERETO_DEBUG_LEVEL=1` as soon as possible.

If you can't find the logs or you are having a hard time with this you can request [Extra Support](https://chevereto.com/support) so we can safely debug your installation.
