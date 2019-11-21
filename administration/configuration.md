# Configuration


Certain settings of the FLAT runner can be configured using environment variables.
The chapter [Defining Env Vars](/cookbook/envvars.md#defining-env-vars) in the Cookbook provides information on how to set environment variables.


## Environment Variables

### Miscellaneous

* `FLAT_SERVER_ROLE`: This setting is typically used to distinguish production systems from staging or development servers. Its value is accessible in the [`$server` variable](/reference/variables.md#predefined-variables) as `$server/role`.


### Request Timeouts

Note that the maximum allowed overall time for a given request is always limited by FLAT_MAX_TIMEOUT. All other timeouts can be individually adjusted for each request using the appropriate [request option](/reference/actions/request.md#options).

Use the following environment variables to configure the timeouts used during requests:

* `FLAT_DEFAULT_TIMEOUT`: Default timeout in seconds for outgoing HTTP requests.
* `FLAT_DEFAULT_CONNECT_TIMEOUT`: Default connection timeout in seconds for outgoing HTTP requests. If set to 0, the value of `FLAT_DEFAULT_TIMEOUT` is used.
* `FLAT_DEFAULT_TTFB_TIMEOUT`: Maximum time in seconds for receiving the first byte.
* `FLAT_MAX_TIMEOUT`: Maximum allowed time in seconds for outgoing HTTP requests.
* `FLAT_MAX_TIMEOUT_PROCESSES`: Specifies how many PHP-FPM processes are allowed to wait for slow sources at any one time. The lower the threshold value, the earlier slow sources will be blocked.

### PHP-FPM

Some parameters used for PHP-FPM process management can be adjusted using environment variables. Refer to the [PHP-FPM documentation](https://www.php.net/manual/en/install.fpm.configuration.php) for more information.

* `FLAT_FPM_MAX_PROCESSES`: Controls `pm.max_children`. Default `100`. The maximum number of child processes to be created.
* `FLAT_FPM_MAX_REQUESTS`: Controls `pm.max_requests`. Default `500`. Limits the number of requests each child process executes before it is replaced by a new process.

## See also

* [Setting Envvars](/cookbook/envvars.md#defining-env-vars) (Cookbook)
