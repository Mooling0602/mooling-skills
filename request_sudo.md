# request_sudo

Run a shell command with `sudo`, popping up a GUI password dialog (`ksshaskpass`) for authentication.

# Install

Refers to [README](README_en_US.md).

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

Especailly in **NixOS**, the `ksshaskpass` is not in `/usr/bin/` but still avaliable, you should find it using `command -v <cmd>`, like this:

```fish
mooling@mooling-laptop ~> command -v ksshaskpass
/etc/profiles/per-user/mooling/bin/ksshaskpass
```

Then use the correct path.

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

Git over SSH authentication can also use `ksshaskpass`:
```bash
SSH_ASKPASS=/usr/bin/ksshaskpass SSH_ASKPASS_REQUIRE=force git pull
```

- This is useful when `ssh` needs a key passphrase during Git operations.
- `SSH_ASKPASS_REQUIRE=force` makes OpenSSH use the askpass dialog even when a terminal is available.

## Notes

- Assembles the full command from the user's request — no script wrapper needed.
- Always verify the command is safe before executing with elevated privileges.
