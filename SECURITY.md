# Security

## What Safecall touches

- It reads files you point Claude Code at, and copies the ones about to be changed.
- Copies go to `~/.safecall/` on your own machine. Nothing leaves the computer: there is no
  network call anywhere in this plugin, and no dependency that could make one.
- The only files it writes outside `~/.safecall/` are the ones Claude Code was already going to
  write — Safecall does not write your documents, it copies them first.

## By design

- **Files whose name mentions a secret are never copied.** `.env`, anything matching
  `secret`, `password`, `credential`, `token`, `id_rsa`, `id_ed25519`, `.pem`, `.key`, `.p12`,
  `.keystore`, `.netrc`, `.htpasswd`. A copy of a secret is a second secret, in a folder that is
  not guarded the way the original may be.
- **Symbolic links are not followed or copied.** A link is not the file.
- **Size limits are hard:** 5 MB a file, 50 MB a copy.
- **Copies expire:** 14 days, or 20 copies per folder, whichever comes first.

## The guard hook

`scripts/guard.py` runs before `Write`, `Edit`, `MultiEdit` and `NotebookEdit`. Before an existing
file is changed it makes a copy of it and lets the write through. It stops the write in one case:
the copy could not be made, so the change would be one-way; the person is asked, and a yes lets the
repeated edit through. New files and reading are untouched. On any trouble of its own — python
missing, timeout, unreadable input — it lets the write through, so the session always runs.

## Reporting a vulnerability

Write to polyhelper.ai@gmail.com with `safecall` in the subject. Keep the details out of public
issues until a fix is released.
