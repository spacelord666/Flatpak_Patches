The Linux entry for Sublime Text used `"Sublime Text"` as the `openWith`
value, which is a display name — not an executable. The correct binary
name is `"subl"`. This caused `execFile` to fail with ENOENT whenever a
user selected "Sublime Text" from the "Open with" menu.

When `resolveAppPath` returns a `flatpak run <app-id>` string, the
`open-path` handler must invoke `flatpak run --file-forwarding <app-id>
<path>` instead of passing the flatpak command string directly to
`execFile`.

and a pretty serious bug here:

`checkAppExists` always returned `true` on Linux without actually checking
if the app was installed. `resolveAppPath` returned the app name as-is,
but flatpak-installed apps need `flatpak run <app-id>` instead of a bare
binary name, since flatpak export directories often aren't on Electron's
PATH.
