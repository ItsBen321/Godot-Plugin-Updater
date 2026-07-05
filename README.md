# Plugin Updater

Plugin Updater is a small Godot editor script that installs and updates addons from GitHub.

It reads GitHub repository links from `addons/plugin-updater/PLUGINS.md`, downloads each repository, finds addon folders that contain a `plugin.cfg`, and copies them into `res://addons`.

## Requirements

- Godot 4
- `git` available on your system path
- `curl` available on your system path, or `wget` on Linux/macOS

On Windows, `curl` is required.

## Install

Copy this folder into your project:

```text
addons/plugin-updater
```

The updater is an `EditorScript`, not an editor plugin, so it does not need to be enabled in Project Settings.

## Configure

Open:

```text
addons/plugin-updater/PLUGINS.md
```

Add one GitHub repository URL per line:

```text
https://github.com/user/example-addon
https://github.com/another-user/another-addon
```

Each repository should contain an installable Godot addon under an `addons/` folder:

```text
addons/example_addon/plugin.cfg
```

If a repository's source checkout is not directly installable, the updater will try the latest GitHub release zip and search it for addon folders.

## Run

In the Godot editor, open:

```text
addons/plugin-updater/plugin-updater.gd
```

Run the script from the script editor.

The updater will:

- disable currently enabled editor plugins
- download each plugin listed in `PLUGINS.md`
- install missing plugins into `res://addons`
- update plugins that already exist
- refresh the editor filesystem when safe
- re-enable plugins that were enabled before the run

Newly installed plugins are left disabled. Enable them from Godot's plugin settings after the install finishes.

## GDExtension Plugins

GDExtension plugins may need an editor restart after updating. When the updater changes a GDExtension plugin, it avoids reloading that plugin in the same editor process and saves the plugin state for the next startup.

If a GDExtension plugin does not appear immediately after updating, restart Godot.

## Notes

- Only GitHub repository URLs are supported.
- Empty lines in `PLUGINS.md` are ignored.
- Avoid comments or bullet points in `PLUGINS.md`; use plain URLs only.
- Download sessions are stored under Godot's `user://plugin-updater-downloads` folder.
- Temporary backups are stored under `user://plugin-updater-backups`.
