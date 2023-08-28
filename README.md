# Ephemeral

Chromium is a big offender of [XDG Base Directory specification](https://specifications.freedesktop.org/basedir-spec/basedir-spec-latest.html) and sprawled a numerous dynasty of Electron apps that inherited this problem. The issue is that Chromium and Electron apps inconsistently store cache files alongside configuration making them difficult to manage and archive.

## Use Cases

This script finds cache files under the user directory that are outside the expected cache directory, moves them to `$XDG_CACHE_HOME` and create symbolic links in their previous paths. This works around [Chromium](https://bugs.chromium.org/p/chromium/issues/detail?id=129861) and [Electron](https://github.com/electron/electron/issues/8124) deficiencies. 

### Moving to memory

Alternatively cache files can be moved to `$XDG_RUNTIME_DIR` which is mounted on memory ([tmpfs](https://www.kernel.org/doc/html/latest/filesystems/tmpfs.html)).

Advantages:

- improves performance 
- prevent the cache size from growing to infinity

Disadvantages: 

- cold load after a shutdown
- changes are not persistent
- can be problematic (in theory at least)

## Usage

Bellow are variables that can be exported and their default values.

- `EPHEMERAL_SRC=$HOME`: sets the source path from where the script will look for Chromium instances.
- `EPHEMERAL_DST=$XDG_CACHE_HOME`: sets the destination path to where the script will link the instances caches.
- `EPHEMERAL_REMOVE_SRC=false`: by default the script will move caches to their new destination before linking, this removes then instead.
- `EPHEMERAL_DRY_RUN=false`: print changes instead of applying them.
- `EPHEMERAL_DEBUG=true`: makes the script output notices instead of only errors. It will be enabled by default when running on a terminal. 
- `EPHEMERAL_DEBUG=false`: makes the script super verbose and is only useful for development.

## Warning

It's generally safe to remove cache files when the programs are not running. Please close programs that might be targeted before using this script to avoid potential errors.

Be warned that this script can permanently remove data due to bugs not yet discovered. Always backup your data and be cautious when running scripts you found on the Internet.

## Tips

### Increase max size of $XDG_RUNTIME_DIR

The default max size of the dir is 10% of RAM _(might be different on your system)_ which can be insufficient, specially if moving old caches instead of removing then.

To increase the limit edit `RuntimeDirectorySize` on `/etc/systemd/logind.conf`.
