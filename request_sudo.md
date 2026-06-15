# request_sudo

Run a shell command with `sudo`, popping up a GUI password dialog (`ksshaskpass`) for authentication.

## How it works

```bash
SUDO_ASKPASS=/usr/bin/ksshaskpass sudo -A <command>
```

- `sudo -A` (askpass mode) delegates password collection to `$SUDO_ASKPASS`.
- `ksshaskpass` is a KDE/Qt password dialog.
- If the dialog is dismissed, the command fails with "Request dismissed".

## Fallbacks

If `ksshaskpass` is not available on the system, try these in order:

1. `pkexec <command>` — Polkit GUI dialog
2. Plain `sudo <command>` — terminal prompt (last resort)

## Examples

Install an RPM:
```bash
SUDO_ASKPASS=/usr/bin/ksshaskpass sudo -A rpm -U /path/to/package.rpm
```

Systemctl:
```bash
SUDO_ASKPASS=/usr/bin/ksshaskpass sudo -A systemctl restart nginx
```

Generic:
```bash
SUDO_ASKPASS=/usr/bin/ksshaskpass sudo -A sh -c '<multi-word command>'
```

## Notes

- Assembles the full command from the user's request — no script wrapper needed.
- Always verify the command is safe before executing with elevated privileges.
