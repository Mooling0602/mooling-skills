---
name: request-sudo
description: Run a shell command with root privileges through a GUI password dialog (ksshaskpass with sudo -A) when no interactive terminal is available to type a password. Use for nixos-rebuild, systemctl, package installation, or any elevated command in a GUI-only session; falls back to pkexec then plain sudo.
metadata:
  source: https://github.com/Mooling0602/mooling-skills/tree/main/skills/request-sudo
---

# request-sudo

Run a shell command with `sudo`, popping up a GUI password dialog (`ksshaskpass`) for authentication.

## How it works

```bash
SUDO_ASKPASS=<ksshaskpass path> sudo -A <command>
```

- `sudo -A` (askpass mode) delegates password collection to `$SUDO_ASKPASS`.
- `ksshaskpass` is a KDE/Qt password dialog.
- If the dialog is dismissed, the command fails with "Request dismissed". That is a normal outcome — do not re-pop the dialog and pester the user.

## Locate ksshaskpass first (important)

**Do not assume it lives at `/usr/bin/ksshaskpass`.** On NixOS it is not in `/usr/bin/` but still available; find the real path with `command -v` before using it:

```bash
command -v ksshaskpass
```

Measured on this machine (NixOS):

```
/etc/profiles/per-user/mooling/bin/ksshaskpass
```

Other local facts: `sudo` lives at `/run/wrappers/bin/sudo` and `pkexec` at `/run/wrappers/bin/pkexec`.

## Fallbacks

If `ksshaskpass` is not available on the system, try these in order:

1. `pkexec <command>` — Polkit GUI dialog
2. Plain `sudo <command>` — terminal prompt (last resort; usually impossible without a terminal)

## Examples

NixOS rebuild:

```bash
SUDO_ASKPASS=/etc/profiles/per-user/mooling/bin/ksshaskpass sudo -A nixos-rebuild switch --flake ~/nixos-config#mooling-laptop
```

Install an RPM:
```bash
SUDO_ASKPASS=/etc/profiles/per-user/mooling/bin/ksshaskpass sudo -A rpm -U /path/to/package.rpm
```

Systemctl:
```bash
SUDO_ASKPASS=/etc/profiles/per-user/mooling/bin/ksshaskpass sudo -A systemctl restart nginx
```

Generic (wrap multi-word commands in `sh -c`):
```bash
SUDO_ASKPASS=/etc/profiles/per-user/mooling/bin/ksshaskpass sudo -A sh -c '<multi-word command>'
```

## Git over SSH authentication

`ksshaskpass` also handles SSH key passphrases:

```bash
SSH_ASKPASS=/etc/profiles/per-user/mooling/bin/ksshaskpass SSH_ASKPASS_REQUIRE=force git pull
```

- This is useful when `ssh` needs a key passphrase during Git operations.
- `SSH_ASKPASS_REQUIRE=force` makes OpenSSH use the askpass dialog even when a terminal is available.

## Notes

- Assembles the full command from the user's request — no script wrapper needed.
- Always verify the command is safe before executing with elevated privileges: confirm the target, the path, and the blast radius, especially for `rm`, overwriting writes, and bulk service restarts.
- When a command fails, read the exit code and stderr first to tell apart three cases: the password dialog was cancelled, the command itself failed, and the path was wrong.
