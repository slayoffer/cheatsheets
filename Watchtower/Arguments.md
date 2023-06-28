By default, watchtower will monitor all containers running within the Docker daemon to which it is pointed (in most cases this will be the local Docker daemon, but you can override it with the `--host` option described in the next section). However, you can restrict watchtower to monitoring a subset of the running containers by specifying the container names as arguments when launching watchtower.

```
$ docker run -d \
    --name watchtower \
    -v /var/run/docker.sock:/var/run/docker.sock \
    containrrr/watchtower \
    nginx redis
```

In the example above, watchtower will only monitor the containers named "nginx" and "redis" for updates -- all of the other running containers will be ignored. If you do not want watchtower to run as a daemon you can pass the `--run-once` flag and remove the watchtower container after its execution.

```
$ docker run --rm \
    -v /var/run/docker.sock:/var/run/docker.sock \
    containrrr/watchtower \
    --run-once \
    nginx redis
```

In the example above, watchtower will execute an upgrade attempt on the containers named "nginx" and "redis". Using this mode will enable debugging output showing all actions performed, as usage is intended for interactive users. Once the attempt is completed, the container will exit and remove itself due to the `--rm` flag.

When no arguments are specified, watchtower will monitor all running containers.

## Help[¶](https://containrrr.dev/watchtower/arguments/#help)

Shows documentation about the supported flags.

```
            Argument: --help
Environment Variable: N/A
                Type: N/A
             Default: N/A
```

## Time Zone[¶](https://containrrr.dev/watchtower/arguments/#time_zone)

Sets the time zone to be used by WatchTower's logs and the optional Cron scheduling argument (--schedule). If this environment variable is not set, Watchtower will use the default time zone: UTC. To find out the right value, see [this list](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones), find your location and use the value in *TZ Database Name*, e.g *Europe/Rome*. The timezone can alternatively be set by volume mounting your hosts /etc/localtime file. `-v /etc/localtime:/etc/localtime:ro`

```
            Argument: N/A
Environment Variable: TZ
                Type: String
             Default: "UTC"
```

## Cleanup[¶](https://containrrr.dev/watchtower/arguments/#cleanup)

Removes old images after updating. When this flag is specified, watchtower will remove the old image after restarting a container with a new image. Use this option to prevent the accumulation of orphaned images on your system as containers are updated.

```
            Argument: --cleanup
Environment Variable: WATCHTOWER_CLEANUP
                Type: Boolean
             Default: false
```

## Remove attached volumes[¶](https://containrrr.dev/watchtower/arguments/#remove_attached_volumes)

Removes attached volumes after updating. When this flag is specified, watchtower will remove all attached volumes from the container before restarting with a new image. Use this option to force new volumes to be populated as containers are updated.

```
            Argument: --remove-volumes
Environment Variable: WATCHTOWER_REMOVE_VOLUMES
                Type: Boolean
             Default: false
```

## Debug[¶](https://containrrr.dev/watchtower/arguments/#debug)

Enable debug mode with verbose logging.

Notes

Alias for `--log-level debug`. See [Maximum log level](https://containrrr.dev/watchtower/arguments/#maximum-log-level).
Does *not* take an argument when used as an argument. Using `--debug true` will **not** work.

```
            Argument: --debug, -d
Environment Variable: WATCHTOWER_DEBUG
                Type: Boolean
             Default: false
```

## Trace[¶](https://containrrr.dev/watchtower/arguments/#trace)

Enable trace mode with very verbose logging. Caution: exposes credentials!

Notes

Alias for `--log-level trace`. See [Maximum log level](https://containrrr.dev/watchtower/arguments/#maximum-log-level).
Does *not* take an argument when used as an argument. Using `--trace true` will **not** work.

```
            Argument: --trace
Environment Variable: WATCHTOWER_TRACE
                Type: Boolean
             Default: false
```

## Maximum log level[¶](https://containrrr.dev/watchtower/arguments/#maximum_log_level)

The maximum log level that will be written to STDERR (shown in `docker log` when used in a container).

```
            Argument: --log-level
Environment Variable: WATCHTOWER_LOG_LEVEL
     Possible values: panic, fatal, error, warn, info, debug or trace
             Default: info
```

## ANSI colors[¶](https://containrrr.dev/watchtower/arguments/#ansi_colors)

Disable ANSI color escape codes in log output.

```
            Argument: --no-color
Environment Variable: NO_COLOR
                Type: Boolean
             Default: false
```

## Docker host[¶](https://containrrr.dev/watchtower/arguments/#docker_host)

Docker daemon socket to connect to. Can be pointed at a remote Docker host by specifying a TCP endpoint as "tcp://hostname:port".

```
            Argument: --host, -H
Environment Variable: DOCKER_HOST
                Type: String
             Default: "unix:///var/run/docker.sock"
```

## Docker API version[¶](https://containrrr.dev/watchtower/arguments/#docker_api_version)

The API version to use by the Docker client for connecting to the Docker daemon. The minimum supported version is 1.24.

```
            Argument: --api-version, -a
Environment Variable: DOCKER_API_VERSION
                Type: String
             Default: "1.24"
```

## Include restarting[¶](https://containrrr.dev/watchtower/arguments/#include_restarting)

Will also include restarting containers.

```
            Argument: --include-restarting
Environment Variable: WATCHTOWER_INCLUDE_RESTARTING
                Type: Boolean
             Default: false
```

## Include stopped[¶](https://containrrr.dev/watchtower/arguments/#include_stopped)

Will also include created and exited containers.

```
            Argument: --include-stopped
Environment Variable: WATCHTOWER_INCLUDE_STOPPED
                Type: Boolean
             Default: false
```

## Revive stopped[¶](https://containrrr.dev/watchtower/arguments/#revive_stopped)

Start any stopped containers that have had their image updated. This argument is only usable with the `--include-stopped` argument.

```
            Argument: --revive-stopped
Environment Variable: WATCHTOWER_REVIVE_STOPPED
                Type: Boolean
             Default: false
```

## Poll interval[¶](https://containrrr.dev/watchtower/arguments/#poll_interval)

Poll interval (in seconds). This value controls how frequently watchtower will poll for new images. Either `--schedule` or a poll interval can be defined, but not both.

```
            Argument: --interval, -i
Environment Variable: WATCHTOWER_POLL_INTERVAL
                Type: Integer
             Default: 86400 (24 hours)
```

## Filter by enable label[¶](https://containrrr.dev/watchtower/arguments/#filter_by_enable_label)

Update containers that have a `com.centurylinklabs.watchtower.enable` label set to true.

```
            Argument: --label-enable
Environment Variable: WATCHTOWER_LABEL_ENABLE
                Type: Boolean
             Default: false
```

## Filter by disable label[¶](https://containrrr.dev/watchtower/arguments/#filter_by_disable_label)

**Do not** update containers that have `com.centurylinklabs.watchtower.enable` label set to false and no `--label-enable` argument is passed. Note that only one or the other (targeting by enable label) can be used at the same time to target containers.

## Without updating containers[¶](https://containrrr.dev/watchtower/arguments/#without_updating_containers)

Will only monitor for new images, send notifications and invoke the [pre-check/post-check hooks](https://containrrr.dev/watchtower/lifecycle-hooks/), but will **not** update the containers.

Note

Due to Docker API limitations the latest image will still be pulled from the registry. The HEAD digest checks allows watchtower to skip pulling when there are no changes, but to know *what* has changed it will still do a pull whenever the repository digest doesn't match the local image digest.

```
            Argument: --monitor-only
Environment Variable: WATCHTOWER_MONITOR_ONLY
                Type: Boolean
             Default: false
```

Note that monitor-only can also be specified on a per-container basis with the `com.centurylinklabs.watchtower.monitor-only` label set on those containers.

## Without restarting containers[¶](https://containrrr.dev/watchtower/arguments/#without_restarting_containers)

Do not restart containers after updating. This option can be useful when the start of the containers is managed by an external system such as systemd.

```
            Argument: --no-restart
Environment Variable: WATCHTOWER_NO_RESTART
                Type: Boolean
             Default: false
```



## Without pulling new images[¶](https://containrrr.dev/watchtower/arguments/#without_pulling_new_images)

Do not pull new images. When this flag is specified, watchtower will not attempt to pull new images from the registry. Instead it will only monitor the local image cache for changes. Use this option if you are building new images directly on the Docker host without pushing them to a registry.

```
            Argument: --no-pull
Environment Variable: WATCHTOWER_NO_PULL
                Type: Boolean
             Default: false
```

## Without sending a startup message[¶](https://containrrr.dev/watchtower/arguments/#without_sending_a_startup_message)

Do not send a message after watchtower started. Otherwise there will be an info-level notification.

```
            Argument: --no-startup-message
Environment Variable: WATCHTOWER_NO_STARTUP_MESSAGE
                Type: Boolean
             Default: false
```

## Run once[¶](https://containrrr.dev/watchtower/arguments/#run_once)

Run an update attempt against a container name list one time immediately and exit.

```
            Argument: --run-once
Environment Variable: WATCHTOWER_RUN_ONCE
                Type: Boolean
             Default: false
```

## HTTP API Mode[¶](https://containrrr.dev/watchtower/arguments/#http_api_mode)

Runs Watchtower in HTTP API mode, only allowing image updates to be triggered by an HTTP request. For details see [HTTP API](https://containrrr.dev/watchtower/http-api-mode).

```
            Argument: --http-api-update
Environment Variable: WATCHTOWER_HTTP_API_UPDATE
                Type: Boolean
             Default: false
```

## HTTP API Token[¶](https://containrrr.dev/watchtower/arguments/#http_api_token)

Sets an authentication token to HTTP API requests.

```
            Argument: --http-api-token
Environment Variable: WATCHTOWER_HTTP_API_TOKEN
                Type: String
             Default: -
```

## HTTP API periodic polls[¶](https://containrrr.dev/watchtower/arguments/#http_api_periodic_polls)

Keep running periodic updates if the HTTP API mode is enabled, otherwise the HTTP API would prevent periodic polls.

```
            Argument: --http-api-periodic-polls
Environment Variable: WATCHTOWER_HTTP_API_PERIODIC_POLLS
                Type: Boolean
             Default: false
```

## Filter by scope[¶](https://containrrr.dev/watchtower/arguments/#filter_by_scope)

Update containers that have a `com.centurylinklabs.watchtower.scope` label set with the same value as the given argument. This enables [running multiple instances](https://containrrr.dev/watchtower/running-multiple-instances).

```
            Argument: --scope
Environment Variable: WATCHTOWER_SCOPE
                Type: String
             Default: -
```

## HTTP API Metrics[¶](https://containrrr.dev/watchtower/arguments/#http_api_metrics)

Enables a metrics endpoint, exposing prometheus metrics via HTTP. See [Metrics](https://containrrr.dev/watchtower/metrics/) for details.

```
            Argument: --http-api-metrics
Environment Variable: WATCHTOWER_HTTP_API_METRICS
                Type: Boolean
             Default: false
```

## Scheduling[¶](https://containrrr.dev/watchtower/arguments/#scheduling)

[Cron expression](https://pkg.go.dev/github.com/robfig/cron@v1.2.0?tab=doc#hdr-CRON_Expression_Format) in 6 fields (rather than the traditional 5) which defines when and how often to check for new images. Either `--interval` or the schedule expression can be defined, but not both. An example: `--schedule "0 0 4 * * *"`

```
            Argument: --schedule, -s
Environment Variable: WATCHTOWER_SCHEDULE
                Type: String
             Default: -
```

## Rolling restart[¶](https://containrrr.dev/watchtower/arguments/#rolling_restart)

Restart one image at time instead of stopping and starting all at once. Useful in conjunction with lifecycle hooks to implement zero-downtime deploy.

```
            Argument: --rolling-restart
Environment Variable: WATCHTOWER_ROLLING_RESTART
                Type: Boolean
             Default: false
```

## Wait until timeout

Timeout before the container is forcefully stopped. When set, this option will change the default (`10s`) wait time to the given value. An example: `--stop-timeout 30s` will set the timeout to 30 seconds.

```
            Argument: --stop-timeout
Environment Variable: WATCHTOWER_TIMEOUT
                Type: Duration
             Default: 10s
```

## TLS Verification[¶](https://containrrr.dev/watchtower/arguments/#tls_verification)

Use TLS when connecting to the Docker socket and verify the server's certificate. See below for options used to configure notifications.

```
            Argument: --tlsverify
Environment Variable: DOCKER_TLS_VERIFY
                Type: Boolean
             Default: false
```

## HEAD failure warnings[¶](https://containrrr.dev/watchtower/arguments/#head_failure_warnings)

When to warn about HEAD pull requests failing. Auto means that it will warn when the registry is known to handle the requests and may rate limit pull requests (mainly docker.io).

```
            Argument: --warn-on-head-failure
Environment Variable: WATCHTOWER_WARN_ON_HEAD_FAILURE
     Possible values: always, auto, never
             Default: auto
```