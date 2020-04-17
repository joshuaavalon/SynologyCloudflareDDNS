# 2.0.0

### Features

- Auto detect IPv4 and IPv6 to handle A or AAAA record.
- No long need to edit the script directly.
- Use Cloudflare API token instead of username and password for limiting permissions.
- Update to correct status in Synology.
- Notify user about errors in updating DDNS.

### BREAKING CHANGES

- Cloudflare API token must be used instead of username and password.
- Username should enter Zone ID.
- No longer output to log files.
