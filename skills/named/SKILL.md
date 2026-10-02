---
name: named
description: Check that every file you are about to name in an answer really exists on this person's disk, before you name it. Use it before telling them what is in their folder, before citing a document back at them, and before any answer that says "your folder has…" or "file X says…". Also use it when they ask "where is this file?", "I don't see one like that", "I don't have that".
argument-hint: "[the folder, if it is not the one we are in]"
allowed-tools: Bash(sh "${CLAUDE_PLUGIN_ROOT}/hooks/python.sh" safecall say scripts/paths.py *) PowerShell(${CLAUDE_PLUGIN_ROOT}/hooks/python.ps1 safecall say scripts/paths.py *) Read Glob
---

# Safecall: do not name a file that is not there

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
with one line saying safecall "is paused" because this computer has no working Python 3 yet, tell the
person that in one plain line and go on by hand — never show them a Python error and stop.

The user said: $ARGUMENTS

Answer in the person's language.

## 1. The failure this prevents

You write: *"Your folder has contract-2024.pdf, and it says…"* — and there is no such file. The
sentence is fluent, the name is plausible, and this person has no way to tell. Sometimes they then
go to the bank looking for it.

A named file is a claim about their disk. **Claims about the disk get checked against the disk.**

## 2. Check before you say it

Put your draft answer through the script:

```
sh "${CLAUDE_PLUGIN_ROOT}/hooks/python.sh" safecall say scripts/paths.py --folder "<the folder>" --text "<your draft>"
```

It lists every file name your text contains and says which are really there. Exit 1 means at least
one is not.

You can also simply use `Glob` and read the names yourself — the point is that you looked, not which
tool you used. The script exists because it also finds the case where the file is real but **one
folder further down**, which is the mistake that reads as a lie and is not one.

## 3. What to do with what is missing

**Never quietly drop it and never quietly rename it.** One of three, out loud:

> I made a mistake: there is no file "contract-2024.pdf" in the folder. There is "contract.pdf" —
> did you mean that one?

> You don't have a file like that. It might be in another folder or not saved yet.

> I named it from memory and checked — it's not there. Striking it out.

Admitting it in one line costs nothing. Being caught later costs the whole relationship with the
tool, and this person will not come back to it.

## 4. The same rule for what you claim to have read

If you say "I read it", you must have read it in this conversation. Not "it is the kind of file
that usually says", not a guess from the name. If you did not open it, the receipt says
**"didn't check"** (`/safecall:receipt`), and the sentence says "going by the name", never "it
says".

## 5. Do not turn this into noise

Do not print the check at the person. They do not need a list of verified paths — they need an
answer that is true. Run it, fix what is wrong, and say nothing about having run it, unless
something was wrong.

## If the launcher says there is no Python

Do the check with `Glob` and your own eyes instead — the
obligation is to look, not to run this script.
