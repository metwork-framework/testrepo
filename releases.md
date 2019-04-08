Title: What's new in MetWork 0.6 ?
Date: 2019-04-08 12:00:00
Category: News
Tags: release-notes, 0.6

The 0.6 release of MetWork modules is available !

Let's talk about new features and changes in this release.

## Log Management

We are adding full log management in MetWork modules. This is not completly
finished in this release. But there is a lot of progress inside.

### mflog

First, we completly redesigned `mflog` API now released as an independant product.

It is an opinionated python (structured) logging library built on [structlog](https://www.structlog.org/).

> Structured logging means that you don’t write hard-to-parse and hard-to-keep-consistent prose
> in your logs but that you log events that happen in a context instead.
> - [](https://www.structlog.org/en/stable/why.html)

Full details are available in [the corresponding github repo](https://github.com/metwork-framework/mflog).

In a metwork context, there is nearly no incompatible changes but:

- `MFXXX_LOG_DEFAULT_LEVEL` env variables (see `[log]` section of your `config.ini` is
replaced by `MFXXX_LOG_MINIMAL_LEVEL`)
- if you have an admin module configured, you will find a new log file called `json_logs.log`
(which contains in JSON format all mflog logs which severity is greather or equal than a configured threshold (`WARNING` by default) ; this file content will be sent to configured `mfadmin` module

But the most interesting change is that you can attach custom key/values to log messages.

For example:

```python

from mflog import get_logger

# Get a logger ("foo.bar" is the logger name of your choice)
log = get_logger("foo.bar")

# [...]

# Bind some attributes to the logger depending on the context
log = log.bind(user="john")
log = log.bind(user_id=123)

# [...]

# Log something
log.warning("user logged in", happy=True, another_key=42)
```

On your `stderr` log, you will get something like this:

```
2019-01-28T07:52:42.903067Z  [WARNING] (foo.bar#7343) user logged in {another_key=42 happy=True user=john user_id=123}
```

And, on your `json_logs.log` (so on your `mfadmin` module), you will get something like that:

```json
{
    "timestamp": "2019-01-28T08:16:40.047710Z",
    "level": "warning",
    "name": "foo.bar",
    "pid": 29317,
    "event": "user logged in",
    "another_key": 42,
    "happy": true,
    "user": "john",
    "user_id": 123
}
```

Note you have now a new `mflog_override.conf` file in `config` directory of each module
to easily override minimal severity level by logger names (wildcards available).

### mfadmin

Of course, big corresponding changes have been made on the `mfadmin` module.

First of all, we separated the `monitoring` layer in two:

- a `metrics` layer (with `influxdb` and `grafana` stuff)
- a `logs` layer (with `elasticsearch` and `kibana` stuff)

Of course, you can choose to install the layers (and corresponding features) you want.

As said in introduction, the `logs` layer works but is not fully completed in this
release.

### Centralization of logs

In this release, you can centralize `mfserv` nginx access logs to a given `mfadmin` module.
To do that, you have to set `[admin]/send_nginx_logs=1` in your `config.ini` and restart.

In the next release (this is already available in `master` branch), you will also be able
to send `mflog` logs for all modules.

## Compatibility with `ksh` and `zsh`

Thanks to the new [mfutil_c](https://github.com/metwork-framework/mfutil_c) project,
the `MetWork` profile is now compatible with alternate shells like `ksh` or `zsh`.

## Work on plugin names

In 0.5 release, a given plugin name was stored in 3 differents files/dirs:

- in the plugin directory name
- in the plugin `config.ini`
- in the `.layerapi2_label` file

So it was difficult and error prone to rename an existing plugin.

In this release, we dropped the `name` key in plugin `config.ini`. In the next
release, we will keep only one reference (the `.layerapi2_label` file).

## nodejs plugins

- nodejs clustering
- nodets

## mfdata

- autostart = True
- drop plugin:autostart
- switch add no_match_policy_keep_tags options
- start of the doc effort
- catch switch magic exception "magic_exception"

## Misc

### Build images

We updated or CentOS 6 and 7 build images to latest releases. And we also added
a new C++ compiler on CentOS6 to be able to compile new GEOS update.

### Dependencies additions and updates

- GEOS update (3.6.2 => 3.7.1)
- add `sphinx-automodapi`

### `python3_ia` layer moved as an external addon

No change for users but the source code is not available in `mfext` module
anymore. It is now available on its dedicated github repo.

### HTTP Timeout management in `mfserv`

There are now two easy ways to configure an http timeout in `mfserv` without writing
a custom nginx configuration fragment:

- the `proxy_timeout` key in your plugin `config.ini`
- the `[nginx]/timeout` key in the module configuration file

### Outdated environment security

There is now a blocking message to avoid the mistake to interactively relaunch
a metwork module with an outdated environment.
