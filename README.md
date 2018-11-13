# Astroid flatpak manifest

This repository contains the flatpak manifest for the stable version. It is
based on the gnome sdk and runtime.

## Building

Install the gnome sdk and runtime:
```
flatpak remote-add --from gnome https://sdk.gnome.org/gnome.flatpakrepo
flatpak install flathub org.gnome.Platform//3.30 org.gnome.Sdk//3.30
```

Build the flatpak package:
```
flatpak-builder --repo=repo build-dir org.astroidmail.astroid.json
```

Install the package:
```
flatpak --user remote-add --no-gpg-verify astroid-repo repo
flatpak --user install astroid-repo org.astroidmail.astroid
```

## Running

For running astroid:
```
flatpak run org.astroidmail.astroid
```

## Configuration

Note that following flatpak's convention, the configuration directory for
astroid is `$HOME/.var/app/org.astroidmail.astroid/config/astroid`.

For calling host commands from within the astroid sandbox, prepend the command
with `flatpak-spawn --host`. E.g. in astroid's config
```
accounts.myAccount.sendmail: "flatpak-spawn --host msmtp --read-envelope-from -i -t"
editor.cmd: "flatpak-spawn --host urxvt -e nvim %1"
editor.markdown_processor: "flatpak-spawn --host pandoc -f markdown -t html -s"
attachment.external_open_cmd: "flatpak-spawn --host xdg-open"
```
or in the `poll.sh` script
```
flatpak-spawn --host offlineimap
```
