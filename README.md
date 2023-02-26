# Ephemeral

Chrome is a big offender of [XDG Base Directory specification](https://specifications.freedesktop.org/basedir-spec/basedir-spec-latest.html) and sprawled a numerous dynasty of Electron apps that don't fare any better.
Chrome store cache files alongside configuration making it difficult to manage and archive.

In modern Linux desktop `/tmp` and `/run` are mounted in virtual memory [tmpfs](https://www.kernel.org/doc/html/latest/filesystems/tmpfs.html). As suck it makes senses to store cache files in one of those as to:

- improve performance
- prevent the cache size from growing to infinity
- with the disadvantage of a cold load after a shutdown

This script do so by moving such files to `/run/user/id` (a.k.a. `$XDG_RUNTIME_DIR`) and symbolic linking them to their previous paths.
