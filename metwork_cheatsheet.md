# MetWork cheatsheet

## foo

- `{mfxxx}`: means a metwork module with services, replace with `mfserv`, `mfdata`, `mfbase`, `mfadmin` or `mfsysmon`

## `root` service commands

As `root` unix user:

| Command | Description |
| --- | --- |
| `service metwork start` | start all installed metwork services |
| `service metwork stop` | stop all installed metwork services |
| `service metwork status` | check all installed metwork services |
| `service metwork start {mfxxx}` | start metwork services for the given metwork module (only) |
| `service metwork stop {mfxxx}` | stop metwork services for the given metwork module (only) |
| `service metwork status {mfxxx}` | check metwork services for the given metwork module (only) |
| `/etc/rc.d/init.d/metwork [...]` | same behaviour than `service metwork [...]` commands (use it if you don't have `service`command on your distribution | 

## root files or directories

| Path | Description |
| --- | --- |
| `/etc/metwork.config.d/{mfxxx}/config.ini` | override the `mfdata` module configuration at system level |
| `/etc/metwork.config.d/{mfxxx}/external_plugins/` | put some `.plugin` files here and they will be installed during `mfserv` service startup |

## module commands

As `{mfxxx}` user:

| Command | Description |
| --- | --- |
| `{mfxxx}.start` | start metwork services for the corresponding metwork module |
| `{mfxxx}.stop` | stop metwork services for the corresponding metwork module |
| `{mfxxx}.status` | check metwork services for the corresponding metwork module |
| `layers`| FIXME |
| `components` | FIXME | 

## plugins management commands

As `mfserv`, `mfdata` or `mfbase` user:

| Command | Description |
| --- | --- |
| `plugins.list` | list installed plugins for the corresponding metwork module |
| `plugins.install /full/path/file.plugin` | install
| `plugin_env {plugin_name}` | enter the given plugin environment |

As `{mfxxx}` user inside the plugin directory (you must have 
