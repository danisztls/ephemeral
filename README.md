# Ephemeral

Chrome is a big offender of [XDG Base Directory specification](https://specifications.freedesktop.org/basedir-spec/basedir-spec-latest.html) and sprawled a numerous dynasty of Electron apps that don't fare any better.
Chrome store cache files alongside configuration making it difficult to manage and archive.

In modern Linux desktop `/tmp` and `/run` are mounted in virtual memory [tmpfs](https://www.kernel.org/doc/html/latest/filesystems/tmpfs.html). As suck it makes senses to store cache files in one of those as to:

- improve performance
- prevent the cache size from growing to infinity
- with the disadvantage of a cold load after a shutdown

This script do so by moving such files to `/run/user/id` (a.k.a. `$XDG_RUNTIME_DIR`) and symbolic linking them to their previous paths.

## Usage
Bellow are variables that can be exported and their default values.

- `EPHEMERAL_SRC=$HOME`: sets the source path from where the script will look for Chromium instances.
- `EPHEMERAL_DST=$XDG_RUNTIME_DIR/ephemeral-cache`: sets the destination path to where the script will link the instances caches.
- `EPHEMERAL_REMOVE_SRC=false`: by default the script will move caches to their new destination before linking, this removes then instead.
- `EPHEMERAL_DRY_RUN=false`: print changes instead of applying them.
- `EPHEMERAL_DEBUG=false`: makes the script super verbose and is only useful for development.

## Warning
It's generally safe to remove cache files when the programs are not running. Please close programs that might be targeted before using this script to avoid potential errors.

Be warned that this script can permanently remove data due to bugs not yet discovered. Always backup your data and be cautious when running scripts you found on the Internet.

## Tips
### Increase max size of $XDG_RUNTIME_DIR
The default max size of the dir is 10% of RAM _(might be different on your system)_ which can be insufficient, specially if moving old caches instead of removing then.

To increase the limit edit `RuntimeDirectorySize` on `/etc/systemd/logind.conf`.
