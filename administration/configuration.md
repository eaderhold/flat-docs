# Configuration


Certain settings of the FLAT server can be configured using environment variables.
The chapter [Defining Env Vars](/cookbook/envvars.md#defining-env-vars) in the Cookbook provides information on how to set environment variables.


## Environment Variables

### Miscellaneous

* `FLAT_SERVER_ROLE`: This setting is typically used to distinguish production systems from staging or development servers. Its value is accessible in the [`$server` variable](/reference/variables.md#predefined-variables) as `$server/role`.
* `FLAT_STATUS_AUTH`: Username and password, separated by a `:` for access to the `php-fpm` and `httpd` status pages. The pages are disabled entirely if `FLAT_STATUS_AUTH` is not set. If enabled, the `httpd` status can be requested via HTTP at `/ServerStatus`, and the php-fpm status at `/FPMStatus?full`.
* `FLAT_DEBUG_ALLOW_HEADER`: enable [per request debugging](/reference/debugging.md#request-debugging), defaults to `false` unless `FLAT_DEBUG_AUTH` is set.
* `FLAT_DEBUG_AUTH`: sets a password to protect [per request debugging](/reference/debugging.md#request-debugging).
* `FIT_FLAT_UID_FORMAT`: use `uuid3` or `uuid4` to switch the format of the `requestID` generated in the logs and the `id` in the DC to UUID version 3 or 4 respectively instead of the Apache httpd unique_id used by default.

### Request Timeouts

Note that the maximum allowed overall time for a given request is always limited by `FLAT_MAX_TIMEOUT`. All other timeouts can be individually adjusted for each request using the appropriate [request option](/reference/actions/request.md#options).

Use the following environment variables to configure the timeouts used during requests:

* `FLAT_DEFAULT_TIMEOUT`: Default timeout in seconds for outgoing HTTP requests.
* `FLAT_DEFAULT_CONNECT_TIMEOUT`: Default connection timeout in seconds for outgoing HTTP requests. If set to 0, the value of `FLAT_DEFAULT_TIMEOUT` is used.
* `FLAT_DEFAULT_TTFB_TIMEOUT`: Maximum time in seconds for receiving the first byte.
* `FLAT_MAX_TIMEOUT`: Maximum allowed time in seconds for outgoing HTTP requests.
* `FLAT_MAX_TIMEOUT_PROCESSES`: Specifies how many PHP-FPM processes are allowed to wait for slow sources at any one time. The lower the threshold value, the earlier slow sources will be blocked.

### php.ini

Arbitrary directives can be merged into `php.ini` by setting environment variables starting with `PHP_INI_`. For example, to set `post_max_size = 100M`, just export `PHP_INI_post_max_size=50M` in your shell before starting `flat` CLI. Refer to the [PHP documentation](https://www.php.net/manual/en/ini.list.php) for a list of directives.

### PHP-FPM

Some parameters used for PHP-FPM process management can be adjusted using environment variables. Refer to the [PHP-FPM documentation](https://www.php.net/manual/en/install.fpm.configuration.php) for more information.

* `FLAT_FPM_MAX_PROCESSES`: Controls `pm.max_children`. Default `100`.
* `FLAT_FPM_MAX_REQUESTS`: Controls `pm.max_requests`. Default `500`. Limits the number of requests each child process executes before it is replaced by a new process.
* `FLAT_FPM_MIN_SPARE`: Controls `pm.min_spare_servers`. Default `5`. The desired minimum number of idle child processes to be created.
* `FLAT_FPM_MAX_SPARE`: Controls `pm.max_spare_servers`. Default `7`. The desired maximum number of idle child processes.
* `FLAT_FPM_START_SERVERS`: Controls `pm.start_servers`. The default is the average of `pm.min_spare_servers` and `pm.min_spare_servers`. The initial number of child processes to be started.

## See also

* [Setting Envvars](/cookbook/envvars.md#defining-env-vars) (Cookbook)

