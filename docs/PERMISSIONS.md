# The permission model

Kobel's whole job is to answer one question for every file on your machine:
*what may the AI do with this?*

## The five levels

| Level | What the AI may do | Typical use |
|---|---|---|
| **Edit** | May edit the file directly | A working folder you are actively building with the assistant |
| **Edit + backup** | May edit; a copy of the previous version is written automatically before every change | Anything you would be annoyed to lose, but still want edited in place |
| **Edit copy** | Works on a copy; the original is never touched | Letting an assistant restructure something you are not ready to hand over |
| **Read only** | May read; may not change anything | Reference material, contracts, archives |
| **Blocked** | Cannot see the file at all | Everything else |

The default for anything not granted is **blocked**. Access is something you add,
not something you take away.

## Per file, not only per folder

A level can be set on a single file, on a folder, or on a whole drive root. This
matters more than it sounds: the common real-world case is a project folder where
one subfolder holds credentials or client data. With folder-only tools you either
expose it or you split the project. Here you set the folder to *Edit* and that one
subfolder to *Blocked*.

## Inheritance

Everything inside a folder carries the folder's level — **including files added
later**. If a folder is set to *Read only* and you drop a new file into it
tomorrow, that file is read-only from the moment it lands.

This rule exists because the alternative fails silently. A file that inherits
nothing is a file the assistant cannot open, in the middle of a task, for reasons
neither of you can see.

## Mixed folders

A folder whose files carry different levels is displayed as **mixed**. It is never
shown as a single level its contents do not actually have.

The reason: a folder shown in one colour is a promise about what is inside it. A
folder that inherited a colour its files never got is a broken promise, and it
breaks in the direction of *you thought that was blocked*.

## Blocked is absence, not filtering

A blocked file does not appear in a directory listing, is not returned by a search,
and cannot be read. The assistant is not shown the file and asked to be careful
with it — the file is not part of what it can address at all.

## Changing your mind mid-session

Levels can be changed while an assistant is connected. The new level applies to the
next tool call. Nothing needs to be restarted, and no session has to be ended.

## What is written down

Every call is logged locally: timestamp, which assistant, which tool, which path,
and the outcome — including calls that were refused. A refused call is often the
more interesting entry.

---

Back to the [README](../README.md).
