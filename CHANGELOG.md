# Changelog

## 0.1.8 — 2026-10-03

- Wording: no disclaimers. Every text the person or the model reads says what Safecall does, as a
  capability: no "not legal advice" line under a contract reading, no "does nothing for now", no
  apology in the no-Python and no-copy messages. The "where it goes" sentence in `/safecall:before`
  is one fact plus the person's choice; the line for an answer someone will show to an office now
  says where the official proof comes from and how to ask for it in writing. The step-0 line reads
  "<plugin> is paused: it starts working the moment this computer has Python 3". The README's
  privacy section is "Your disk, your choice"; SECURITY.md is the dry minimum and matches what the
  guard actually does (copies, and stops a write only when no copy can be made). The guard's logic is
  unchanged. New test `tests/test_no_disclaimers.py` keeps stop phrases out of every shipped text.

## 0.1.7 — 2026-10-02

- On Windows the step-0 launcher (`hooks/python.ps1`, and `hooks/python.sh` in Git Bash) also finds a Python installed
  after Claude Code started, with no restart: Claude Code hands its hooks and shells the PATH it was started with, so
  python.org's fresh Python is not on it. After `python`, `py -3` and `python3` on that PATH the launcher now looks at the
  `py` launcher (`%LOCALAPPDATA%\Programs\Python\Launcher`, `%SystemRoot%`), `%LOCALAPPDATA%\Programs\Python\Python3*`
  (and `%ProgramFiles%\Python3*`), newest first, and the install paths in the registry
  (`HKCU`/`HKLM\Software\Python\PythonCore\*\InstallPath`). Same proof as before: a candidate counts only once `-c`
  says 3.8+, and the Microsoft Store stub is never started. The step-0 line no longer asks for a restart. Checked on
  GitHub Actions windows-latest: Python installed silently in the middle of a job, then a hook (PowerShell 7 and 5.1),
  a skill's script and Git Bash ran on it with the PATH unchanged.

## 0.1.6 — 2026-10-02

- Skills run their scripts through the step-0 launcher, on every system: the Bash tool runs
  `sh "${CLAUDE_PLUGIN_ROOT}/hooks/python.sh" safecall say scripts/<name>.py ...`, the PowerShell tool (Windows without
  Git Bash) the same line starting with the bare path `${CLAUDE_PLUGIN_ROOT}/hooks/python.ps1`. No skill calls `python3`
  any more (on Windows it is often missing or the Microsoft Store stub). The launcher takes a Python only once `-c`
  proves 3.8+, tries `python`, `py -3`, `python3` on Windows, never starts the Store or Apple stub, and with no Python
  says one step-0 line. Scripts are passed relative to the plugin root; text piped in PowerShell reaches the script as
  UTF-8 with no BOM (Windows PowerShell 5.1 wrote one). `allowed-tools` grant both forms, quoted as the command is -
  the old unquoted `Bash(python3 ${CLAUDE_PLUGIN_ROOT}/...)` never matched the quoted commands and always prompted.
  Checked on GitHub Actions on windows-latest, macos-latest and ubuntu-latest, including a real `claude -p` that opens a
  skill and runs its command with no prompt, through Bash and through PowerShell.
- The guard's one refusal (a copy could not be made) is now the documented PreToolUse `deny` on stdout: Claude
  Code shows the reason alone instead of the whole two-line hook command in front of it.

## 0.1.5 — 2026-10-02

- Hooks on Windows without Git Bash. There Claude Code runs hook commands in PowerShell, where `sh` does not
  exist, so the hooks failed. Each command in `hooks/hooks.json` is now two lines: sh and Git Bash run the first
  (`exec sh hooks/python.sh ...`, which never comes back); PowerShell finds no `exec`, goes on to the second (whose
  hoisted `trap { continue }` keeps it quiet about that) and
  loads `hooks/python.ps1` - the same step 0 guard (a real Python 3.8+ or one line and exit 0), as a script block
  so no execution policy stops it, with UTF-8 both ways. Checked on GitHub Actions on windows-latest (PowerShell 7,
  Windows PowerShell 5.1, Git Bash; with Python and without), macos-latest and ubuntu-latest, by a real
  `claude -p` session and by every hook command run the way Claude Code spawns it.
- `hooks/python.sh` exports `PYTHONUTF8=1`, so on Windows a hook reads non-English file names right.
- README: install from Poly A1's catalogue by its raw link (`/plugin marketplace add https://raw.githubusercontent.com/vadimchernets/poly-a1-plugins/main/.claude-plugin/marketplace.json`,
  then `/plugin install safecall@poly-a1`) - no git needed; the Poly A1 folder is the way without internet,
  and `marketplace remove` is never the way to switch.

## 0.1.4 — 2026-10-02

- Step 0 guard: every hook now runs through `hooks/python.sh`, which starts the hook only with a
  real Python 3.8+. On a Mac without Command Line Tools it never runs the `/usr/bin/python3` stub (the
  one that pops Apple's install window mid-lesson); on Windows it skips the Microsoft Store stub and
  finds python.org's `python` or `py`. With no Python the session start says so in one line and the
  session goes on; nothing errors. Tests: `tests/test_step0.py`.
- Every release now carries `safecall-0.1.4.zip` (one top folder `safecall-0.1.4/`), built by the new
  `scripts/release-zip.sh` and attached by `.github/workflows/release.yml` on each `v*` tag. The Poly A1
  catalogue installs it as an `archive` source with its `sha256`, so installing needs no git: a
  beginner's Linux has none, and on a Mac without Apple's Command Line Tools `git` is the stub that
  opens Apple's install window.

## 0.1.3 — 2026-10-02

- The Poly A1 coach's state file is `NEXT.md` in every language now - the same name Poly A1
  itself uses - instead of a different name per language. `state_file` in every `lang/<code>.json`
  is `NEXT.md`; the old localized names (es, pt, ru, uk) stay listed in a new
  `state_file_legacy` field, recognized for reading only, so a file an existing user already has
  is still found and never blocked.
- `scripts/guard.py` gained `legacy_state_file_names()` and `state_file(folder)`: NEXT.md if it
  exists, else an existing legacy file, else NEXT.md (where a new file is written).
- Tests: every language's `state_file` is `NEXT.md`; the guard never blocks a legacy-named state
  file either; `state_file()` falls back to a legacy file only when NEXT.md is absent.

## 0.1.2 — 2026-10-02

- Every language is equal now: the per-language word tables (the Poly A1 coach's state-file name,
  the secret-name word list) moved out of `scripts/guard.py` and `scripts/snapshot.py` into their
  own `lang/<code>.json` file - one file per language (`en`, `es`, `pt`, `ru`, `uk`), read the same
  way, none of them hardcoded into the Python source as a special case of another. Behaviour is
  unchanged: the same files are still refused, the same state-file names are still never blocked.
- Ukrainian's secret-name words are no longer a lightly-adapted copy of the Russian ones: its own
  spellings of "login" and "PIN code" (distinct from the Russian ones - see `lang/uk.json`) are
  matched alongside the words the two languages do share. Spanish and Portuguese gained
  `pasaporte`/`passaporte` and a safely word-bounded `pin`, so a passport or a PIN code is caught
  in those languages too, not only `.env` and `id_rsa`.
- The old (safecall <= 0.1.0) Russian metadata and note names - the legacy snapshot meta file and
  its keys, the legacy "where we stopped" notes folder and its keys - moved into lang/ru.json's
  `"legacy"` section. `scripts/snapshot.py` and `scripts/state.py` read them from there; reading an
  old copy or note made before the 0.1.0->0.1.1 rename still works exactly as before.
- Added `scripts/check_language.py` (`python3 scripts/check_language.py`): fails if Cyrillic
  appears anywhere outside a language place (a `ru`/`uk` path segment, `README.ru.md`,
  `lang/ru.json`, `lang/uk.json`, or a language's own self-name in a list of languages). Every
  Russian or Ukrainian literal that used to live in `scripts/guard.py`, `scripts/snapshot.py`,
  `scripts/state.py`, `tests/test_safecall.py` and `tests/behavior/run.py` moved into `lang/*.json`
  or `tests/fixtures/ru|uk/` to pass it; the checker itself is covered by
  `tests/test_check_language.py`.
- Tests: added coverage that every `lang/<code>.json` loads and has the shared shape, that one
  natural secret-named sample file per language (en/es/pt/ru/uk) is refused, and that the legacy
  Russian names are still read correctly - alongside the existing round-trip and legacy tests,
  which keep their original assertions unchanged.
- Two stray bytecode files (`tests/__pycache__/*.pyc`) are no longer tracked.

## 0.1.1 — 2026-10-02

- Project language is English: comments, skill instructions, script output and docs translated; skills
  answer in the person's language. Russian stays only as a localization (README.ru.md, the ru/uk entries
  of the secret-name word list, ru test fixtures).
- Runtime names are English now (`snapshot.json` and its keys, the `notes/` folder for "where we stopped",
  `README.txt` in `~/.safecall`). Copies and notes made by 0.1.0 under the old Russian names are still
  read: listed, restored, checked by `covered`, shown as last time's note. Nothing on disk is renamed.
- Fix: restoring an old snapshot could prune that very snapshot before copying out of it.
- The Russian-scenario behaviour suite now lives in `tests/behavior/ru/` as the Russian localization of
  the suite; `run.py --lang` picks a language (default: every language folder present). One checker
  pattern widened so a correct refusal phrased with words in between is no longer marked as a failure.
