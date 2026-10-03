# Safecall

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.23107729.svg)](https://doi.org/10.5281/zenodo.23107729)

**Nothing is written before a copy exists. Nothing is read before you have seen the list. No answer
ends without saying what nobody checked.**

A plugin for [Claude Code](https://claude.com/claude-code), written for the person who is not a
programmer, has a folder of their own documents, and has two reasons not to let an AI near it:

1. *what if it deletes my files*
2. *what if it says something confident and wrong, and I act on it*

Safecall answers both, and adds the three readings such a person actually needs from a document.

## What it does

| | |
|---|---|
| `/safecall:before` | Lists the folder and waits before reading it. Copies every file it is about to change. |
| `/safecall:undo` | Puts the files back the way they were. The undo is itself undoable. |
| `/safecall:show` | Says what will change **before** it changes, in meaning, not in diff lines. Says where things landed after. |
| `/safecall:receipt` | Three lines under an answer: checked · **not** checked · source. |
| `/safecall:traps` | Where the catch is in a contract, loan, policy or estimate — every point with a quote. |
| `/safecall:holes` | What a plan is missing, as three questions, not thirty. |
| `/safecall:facts` | What here can be checked, what is opinion, what says nothing at all. |
| `/safecall:where` | Picks up where the last evening stopped; writes down where this one stopped. |
| `/safecall:clearer` | Turns "umm, that thing with the bank" into a question that can be answered - by guessing two or three versions out loud, not by interrogating. |
| `/safecall:send` | Reads a letter, complaint or reply the way a cold stranger would, **before** it goes: what should not leave, blanks nobody filled in, a tone that will cost them. |
| `/safecall:named` | Checks that every file an answer names really exists on the disk, before naming it — and finds the one that is real but a folder further down. |
| `/safecall:doctor` | Why the AI is silent, in words, with one step at a time. |

Two hooks, both quiet:

- **on session start** — the real date and time of this computer (a model otherwise wishes you good
  night at ten in the morning), and where you stopped last time;
- **before writing to a file that already exists** — the guard **makes the copy itself** and lets
  the write through. Nobody is stopped: a copy costs nothing, so there is nothing to refuse. A new
  file is untouched, reading is untouched. The one refusal is a file that *cannot* be copied (its
  name mentions a password or a key) — then the change would be one-way, and the person is asked.
  The session runs as usual on any machine; the copies switch on wherever Python 3 is installed.

## Install

```
/plugin marketplace add https://raw.githubusercontent.com/vadimchernets/poly-a1-plugins/main/.claude-plugin/marketplace.json
/plugin install safecall@poly-a1
```

The first line adds Poly A1's catalogue by its link - one file, no git and no GitHub account - and
later corrections reach you from the same place (Claude Code 2.1.224 or later; `claude update`). If
`poly-a1` is already there, from the Poly A1 folder or from before, skip it: the second line is enough.

Without internet, from the Poly A1 folder:

```
/plugin marketplace add <path to the Poly A1 folder>
/plugin install safecall@poly-a1
```

Once there is internet, the folder is switched to the link in place, keeping everything installed
([how](https://github.com/vadimchernets/poly-a1-plugins/blob/main/OFFER-THESE.md#later-from-the-folder-to-github-without-losing-anything)). Never `/plugin marketplace remove poly-a1`: it uninstalls every plugin that came from
it and deletes their saved data.

Then, in `/plugin` → Marketplaces, **turn on auto-update** — for marketplaces that are not
Anthropic's own it is off by default, and without it you stay on the version you installed.

## Where the copies live

`~/.safecall/copies/`, outside your own folder, with a note in it saying not to delete it.
The name is ASCII on purpose: a folder named in Cyrillic or another non-Latin script cannot be
typed by somebody on a different keyboard layout when support tells them to open it.
A copy sitting next to your documents gets mistaken for clutter and thrown away by the very person
it protects. Copies older than 14 days remove themselves; at most 20 are kept per folder; nothing
larger than 5 MB a file or 50 MB a copy.

**Files whose name mentions a password or a key are never copied** — a copy of a secret is a second
secret, and this folder is not guarded the way the original may be.

## Your disk, your choice

**Safecall itself** makes no network call, has no account, no key, and installs no second program.
Every file it writes is on your own disk and you can open all of it. Python 3.8+ and nothing else.

What you give Claude Code goes to Anthropic to be read — that is how Claude Code works. Safecall
makes every changed file returnable and shows you the list before anything is read, so you choose
what goes in: `/safecall:before` names the folders worth keeping to yourself in plain words.

## Requirements

Claude Code with plugin support, and Python 3 on the machine (macOS with Apple's Command Line Tools
and most Linux have it; on Windows `winget install -e --id Python.Python.3.12 --scope user`). The hooks
run on Mac, Linux and Windows - on Windows with Git Bash or, without it, in PowerShell.

## Licence

Apache-2.0. See [LICENSE](LICENSE) and [NOTICE](NOTICE).
