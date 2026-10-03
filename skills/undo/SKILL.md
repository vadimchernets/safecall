---
name: undo
description: Put the person's files back the way they were before you changed them. Use this the moment they say anything like "put it back the way it was", "put it back", "undo that", "you broke it", "undo it", "I don't like it, go back" - and use it before arguing that the change was correct. Also use it when they sound frightened about what just happened to a file.
argument-hint: "[which copy, if not the most recent one]"
allowed-tools: Bash(sh "${CLAUDE_PLUGIN_ROOT}/hooks/python.sh" safecall say scripts/snapshot.py *) PowerShell(${CLAUDE_PLUGIN_ROOT}/hooks/python.ps1 safecall say scripts/snapshot.py *) Read
---

# Safecall: put it back the way it was

## Running safecall's scripts (Mac, Linux, Windows)

Every script command on this page is written for the **Bash** tool and starts with
`sh "${CLAUDE_PLUGIN_ROOT}/hooks/python.sh" safecall say scripts/…`. If your shell tool is **PowerShell** (Windows
without Git Bash), only the start changes: write the launcher's path bare, with no quotes and no `&`
— `${CLAUDE_PLUGIN_ROOT}/hooks/python.ps1 safecall say scripts/…` — and keep the rest, on one line; that is the
form this skill's permission covers. Only if that path has a space in it, write
`& "${CLAUDE_PLUGIN_ROOT}/hooks/python.ps1" …` instead (the person is then asked once). Text for standard input:
`@'…'@ | ${CLAUDE_PLUGIN_ROOT}/hooks/python.ps1 …` (`| & "…"` if the path has a space) instead of `<<'EOF'` — also asked once.
Never call `python3`, `python` or `py` yourself: the launcher finds a real Python 3.8+ (`python`,
then `py -3`, then `python3`) and never starts the Microsoft Store or Apple stub. If it answers
with one line saying safecall "is paused" because this computer has no working Python 3, tell the
person that in one plain line and go on by hand — never show them a Python error and stop.

The user said: $ARGUMENTS

Answer in the person's language. **Do this first and explain afterwards.** A person who says "put
it back the way it was" is frightened, and an explanation before the undo reads as an argument.

## 1. Show what can be put back

```
sh "${CLAUDE_PLUGIN_ROOT}/hooks/python.sh" safecall say scripts/snapshot.py list --folder "<the folder>"
```

Say the list in dates and file names, not in numbers of snapshots:

> I can return things to how they were today at 2:20pm — that's the letter and the spreadsheet.

## 2. Show what would change — and say, in the same breath, that the return is itself returnable

Run it once without `--yes`. That changes nothing and prints exactly which files would come back.

**The question and the reassurance are one message, in this order, and neither goes without the
other.** This is not a style rule. The person is frightened *at the moment you ask*, and a promise
delivered after they have already said yes arrives too late to have been any use:

> These files will come back: "letter-to-bank.docx", "expenses.xlsx" — as they were today at
> 2:20pm.
> What's there now, I'll save before putting them back — **if you change your mind, I'll bring
> it back.**
> Shall we put it back?

Three lines, always all three: **what comes back, by name · the return is undoable · the question.**
If your draft has the question but not the middle line, it is not finished — add the line, do not
send it. Then, once they say yes:

```
sh "${CLAUDE_PLUGIN_ROOT}/hooks/python.sh" safecall say scripts/snapshot.py restore --yes --folder "<the folder>"
```

Add `last` or a number to pick an older copy.

## 3. After the return, say it once more

The script copies the current state before overwriting it, so the sentence is still true afterwards
and still worth saying:

> Put it back the way it was at 2:20pm. What was there before the return, I saved that too — if
> you change your mind, I'll bring it back.

## 4. If there is no copy

Do not hide it and do not blame them. Say it plainly, then say what still exists:

> There is no copy of this file from before the change. An earlier version still lives in <the
> place below> — let's get it from there.

Then look for what is genuinely recoverable and say which of these the person has, in their words:
the Mac's own Time Machine, the Windows "Previous Versions" of a folder, the cloud folder's own
version history (iCloud, OneDrive, Google Drive, Dropbox all keep one), or the file still being
open and unsaved in a program. Name the one that fits their machine and walk them to it.

Then make a copy of whatever is left, immediately, before doing anything else.

## If the launcher says there is no Python

On Windows Python is one command away (step 0), and on a Mac it arrives with Apple's Command Line Tools.
Say that in one plain line and do the job by hand meanwhile — read the folder, say what you are about
to change, change it carefully. A Python error is never shown to the person.

> The automatic copies switch on the moment Python is on this computer — that is step 0, a few
> minutes. Meanwhile I work by hand: before every change I tell you in words what I am changing.
