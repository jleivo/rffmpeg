# Harden SSH Configuration

This directory contains a small set of scripts and configuration files that help
*harden* the SSH daemon on the server **tuvllmsrvp02**. The main goal is
to limit the jellyfin user to use the host only for ffmpeg decoding / encoding and 
nothing more. The scripts and configuration files in this directory implement the 
boundaries. Actions of the jellyfin user are stored in `/var/log/jellyfin_commands.log`

## Contents

| File | Purpose |
|------|---------|
| `limited-wrapper.sh` | Helper script that is called from a restricted SSH session. This defines the limits for the jellyfin user.
| `limited-wrapper-log.conf` | rsyslog configuration for the wrapper’s log output.
| `10-jellyfin-limits.conf` | Additional `sshd_config.d` snippet that restricts access for the Jellyfin user.
| `deploy.sh` | Deploys the above files to the remote server. It performs a **dry‑run** by default and only copies/updates files when invoked with `--apply` (or `--commit`).

## Using `deploy.sh`

The deployment script is safe to run repeatedly. It will:

1. Verify that the target server is reachable.
2. Compare local files with the remote versions using MD5 checksums.
3. Plan the required actions (create directories, copy files, set modes, owners).
4. Show a summary of the planned actions.

```bash
# Show what would happen (dry‑run – this is the default)
./deploy.sh

# Actually apply the changes
./deploy.sh --apply
```

---

*Author*: Juha Leivo\
*Version*: 1.0.0 (2025‑11‑03)
