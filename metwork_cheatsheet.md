# MetWork/{{cookiecutter.name}} cheatsheet

## `root` service commands

As `root` unix user:

| Command | Description |
| --- | --- |
| `service metwork start` | start all installed metwork services |
| `service metwork stop` | stop all installed metwork services |
| `service metwork status` | check all installed metwork services |
{% if cookiecutter.repo not in ['mfext', 'mfcom'] %}
| `service metwork start {{cookiecutter.repo}}` | start {{cookiecutter.repo}} metwork services |
| `service metwork stop {{cookiecutter.repo}}` | stop {{cookiecutter.repo}} metwork services |
| `service metwork status {{cookiecutter.repo}}` | check {{cookiecutter.repo}} metwork services |
{% endif %}

> Note: if you don't have `service` command on your Linux distribution, you can use `/etc/rc.d/init.d/metwork` instead of `service metwork`. For example: `/etc/rc.d/init.d/metwork start` instead of `service metwork start`. If your Linux distribution uses `systemd`component, you can also start metwork services with classic `systemctl` commands.

{% if cookiecutter.repo not in ['mfext', 'mfcom'] %}
## root files or directories

| Path | Description |
| --- | --- |
| `/etc/metwork.config.d/{{cookiecutter.repo}}/config.ini` | override the `mfdata` module configuration at system level |
| `/etc/metwork.config.d/{{cookiecutter.repo}}/external_plugins/` | put some `.plugin` files here and they will be installed during `mfserv` service startup |
{% endif %}

{% if cookiecutter.repo in ['mfext', 'mfcom'] %}

## "load environment" commands

As a "not metwork" unix user:

| Command | Description |
| --- | --- |
| `source /opt/metwork-{{cookiecutter.repo}}/share/interactive_profile` | FIXME |
| `source /opt/metwork-{{cookiecutter.repo}}/share/profile` | FIXME |
| `/opt/metwork-{{cookiecutter.repo}}/bin/{{cookiecutter.repo}}_wrapper {YOUR_COMMAND}`| FIXME |
| `outside {YOUR_COMMAND}`| FIXME |

> Note: if you don't have `/opt/metwork-{{cookiecutter.repo}}` symbolic link, use `/opt/metwork-{{cookiecutter.repo}}-{BRANCH}` instead.

{% endif %}

## module commands

As `{{cookiecutter.repo}}` user:

| Command | Description |
| --- | --- |
{% if cookiecutter.repo not in ['mfext', 'mfcom'] %}
| `{{cookiecutter.repo}}.start` | start {{cookiecutter.repo}} services |
| `{{cookiecutter.repo}}.stop` | stop {{cookiecutter.repo}} services |
| `{{cookiecutter.repo}}.status` | check {{cookiecutter.repo}} services |
{% endif %}
| `layers`| FIXME |
| `components` | FIXME | 

{% if cookiecutter.repo in ['mfserv', 'mfbase', 'mfdata'] %}
## plugins management commands

As `{{cookiecutter.repo}}` unix user:

| Command | Description |
| --- | --- |
| `plugins.list` | list installed plugins |
| `plugins.install /full/path/file.plugin` | install the given plugin file |
| `plugins.uninstall {plugin_name}` | uninstall the given plugin name |
| `plugins.info {plugin_name}` | get some informations about the given plugin name (must be installed) |
| `plugins.info /full/path/file.plugin` | get some informations about the given plugin file (does not need to be installed) |
| `plugin_env {plugin_name}` | enter (interactively) in the given plugin environment |
| `plugin_wrapper {plugin_name} {YOUR_COMMAND}` | execute the given command in the given plugin environment (without changing anything to your current environment) |
{% endif %}

{% if cookiecutter.repo in ['mfserv', 'mfbase', 'mfdata'] %}
## plugins development commands

As `{{cookiecutter.repo}}` unix user:

| Command | Description |
| --- | --- |
| `bootstrap_plugin.py list` | list available plugin templates |
| `bootstrap_plugin.py create --template={TEMPLATE} {PLUGIN_NAME}` | bootstrap a plugin directory `{PLUGIN_NAME}` from the given template  |

As `{{cookiecutter.repo}}` unix user, inside a plugin directory (you must have a `Makefile` and `config.ini` files inside the current working directory):

| Command | Description |
| --- | --- |
| `make develop`| FIXME |
| `make release`| FIXME |
| `make`| FIXME |

{% endif %}


FIXME:

- layer_load
- layer_unload
