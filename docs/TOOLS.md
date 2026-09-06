# The MCP tools Kobel exposes

Every tool is prefixed `kobel_`, every description begins with `[Kobel]`, and every
title reads `Kobel · <name>`. This is deliberate: when an assistant asks you to
approve a call, you can see at a glance which application is asking.

Every call is subject to the permission level of the file it touches, and every
call is logged locally.

## Reading

| Tool | Does |
|---|---|
| `kobel_list_root_folders` | Lists the shared folders with their permission levels and file counts |
| `kobel_list_files` | Lists the contents of a folder. Blocked files do not appear |
| `kobel_read_file` | Reads a file's contents |
| `kobel_read_file_lines` | Reads a line range — the reliable way to inspect a large file before patching it |
| `kobel_read_file_base64_chunk` | Reads a binary file in chunks |
| `kobel_get_file_info` | Size, timestamps, permission level |
| `kobel_search_in_files` | Full-text search. Restrict it to a folder — an unrestricted search over tens of thousands of files is cut off with a marked partial result |
| `kobel_diff_files` | Compares two files |

## Writing

| Tool | Does |
|---|---|
| `kobel_save_file` | Writes a file. The normal way to create or replace a file |
| `kobel_save_file_chunked` | Writes a large file in parts |
| `kobel_write_file` | Low-level write |
| `kobel_patch_text` | Replaces an exact passage. Whitespace must match exactly — read the lines first |
| `kobel_prepare_for_editing` | Prepares a file for editing according to its level (for *Edit + backup* this is where the backup is written; for *Edit copy* it is where the working copy is made) |
| `kobel_create_directory` | Creates a folder |
| `kobel_copy_file`, `kobel_move_file`, `kobel_rename_file` | File management |
| `kobel_delete_file` | Deletes a file |

A write against a *Read only* or *Blocked* file fails. It is not silently skipped
and it is not partially applied.

## Images

Narrow, named image operations, chosen instead of a general script-execution tool:

| Tool | Does |
|---|---|
| `kobel_bild_info` | Format, dimensions, colour space |
| `kobel_bild_skalieren` | Scales |
| `kobel_bild_zuschneiden` | Crops |
| `kobel_bild_ecken_abrunden` | Rounds corners |
| `kobel_bild_konvertieren` | Converts between formats |

## Other

| Tool | Does |
|---|---|
| `kobel_list_projects` | Lists recognised projects |
| `kobel_store_knowledge` | Writes to Kobel's local knowledge store (SQLite) |
| `kobel_hilfe` | Kobel's own help, callable by the assistant |

## What is deliberately absent

There is **no** shell tool, no `run_command`, no `eval`, no script execution. Such
a tool would make every permission level meaningless, because anything the shell
can reach is reachable regardless of what the file rules say. Narrow named
operations are added instead when they are needed — the image tools above exist for
exactly that reason.

## Proxied tools

Connected accounts and third-party MCP servers appear as additional tools under
their own names, routed through Kobel so that they inherit the same approval and
logging behaviour. Which ones exist depends on what you have connected — social
accounts, e-mail, cloud storage, or another MCP server you added yourself.

---

Back to the [README](../README.md).
