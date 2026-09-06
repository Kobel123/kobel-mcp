# Kobel — a permission gateway between AI assistants and your computer

Kobel is a desktop application for **Windows and macOS** that runs a local
[Model Context Protocol](https://modelcontextprotocol.io) server. It sits between
your AI assistant and your machine, and it decides — per file — what the
assistant is allowed to do.

Everything runs locally on your computer. There is no Kobel cloud, and files are
not uploaded to one.

Website: <https://kobel.app/en/>

> **About this repository.** Kobel is a commercial, closed-source desktop app.
> This repository holds its **public documentation**: the permission model, the
> MCP tools it exposes, and the connection guides for each AI client — so that
> they can be read, linked and checked by anyone without installing anything.

---

## The problem

Giving an AI assistant access to your files is usually all-or-nothing. You either
paste content into a chat window by hand, or you hand a filesystem MCP server one
or more directory paths and hope it stays inside them. There is no middle ground
where *this* spreadsheet is read-only, *that* project folder is editable, and the
folder next to it does not exist as far as the assistant is concerned.

Kobel is that middle ground, with a graphical interface instead of a JSON config
file — aimed at people who use AI daily but do not want to edit `mcp.json`.

## The five permission levels

Every file carries exactly one level:

| Level | What the AI may do |
|---|---|
| **Edit** | The AI may edit this file directly. |
| **Edit + backup** | The AI may edit; a backup is created automatically before every change. |
| **Edit copy** | The AI works on a copy, the original stays safe. |
| **Read only** | The AI can see the file but cannot change it. |
| **Blocked** | The AI cannot see this file. |

Two rules make this predictable in practice:

1. **Levels apply per file.** A whole folder can be set at once, and everything
   placed inside it inherits that level — including files added later. A file
   never ends up unprotected because it arrived after the rule was made.
2. **A folder always shows what is actually inside it.** If its files carry
   different levels, it is shown as *mixed* — never as a single colour its
   contents do not have. What you see is the real state, not an inherited label.

Blocked files are not filtered out of an answer after the fact. They are not
visible to the tool layer at all: a listing does not contain them, and a read
request for one fails.

Details: [docs/PERMISSIONS.md](docs/PERMISSIONS.md)

## What the assistant can actually call

Kobel exposes a fixed set of MCP tools. Every tool is prefixed `kobel_`, every
description starts with `[Kobel]`, and every title reads `Kobel · <name>` — so
that in an assistant's approval dialog it is always visible which application is
asking.

**Files** — `kobel_list_root_folders`, `kobel_list_files`, `kobel_read_file`,
`kobel_read_file_lines`, `kobel_read_file_base64_chunk`, `kobel_get_file_info`,
`kobel_search_in_files`, `kobel_diff_files`

**Writing** — `kobel_save_file`, `kobel_save_file_chunked`, `kobel_write_file`,
`kobel_patch_text`, `kobel_prepare_for_editing`, `kobel_create_directory`,
`kobel_copy_file`, `kobel_move_file`, `kobel_rename_file`, `kobel_delete_file`

**Images** — `kobel_bild_info`, `kobel_bild_skalieren`, `kobel_bild_zuschneiden`,
`kobel_bild_ecken_abrunden`, `kobel_bild_konvertieren`

**Other** — `kobel_list_projects`, `kobel_store_knowledge`, `kobel_hilfe`

There is deliberately **no** general shell or script execution tool. Narrow,
named operations were chosen over a generic "run this" escape hatch.

Beyond the file tools, connected accounts and third-party MCP servers appear as
additional namespaced tools — see [App proxy](#app-proxy-and-connected-accounts).

Full list with parameters: [docs/TOOLS.md](docs/TOOLS.md)

## Supported clients

Kobel speaks plain MCP, so any MCP client works. Guided setup exists for:

- **Claude Desktop** — local stdio connection
- **ChatGPT** (the normal chat window) — over an HTTPS tunnel, because OpenAI
  connects to MCP servers from its own cloud, not from your device
- **OpenAI Codex**
- **Cursor**
- **GitHub Copilot / VS Code**
- **Gemini CLI**
- **Ollama** and other local runtimes
- Templates for Qwen Code, Kimi, CodeBuddy, Trae, Cherry Studio, Chatbox,
  Roo Code, Warp and others

Step-by-step per client: [docs/CONNECT.md](docs/CONNECT.md)

## Activity log

Every tool call is written to a local log with a timestamp: which assistant, which
tool, which file, and what came back. Permissions can be withdrawn while a session
is running; the change takes effect on the next call.

## App proxy and connected accounts

Kobel is not limited to files. The same permission layer can be put in front of
other things you want an assistant to reach:

- **Third-party MCP servers**, proxied through Kobel so they inherit its
  approval and logging behaviour
- **Accounts** connected over OAuth — LinkedIn, Instagram, Facebook, Telegram,
  TikTok, e-mail
- **Cloud storage** — Google Drive, Dropbox, OneDrive

Credentials are stored encrypted on the machine.

## Teams

An administrator can publish a policy over a URL or a UNC path. Clients read it
and apply it, which allows folder rules and blocked paths to be set centrally
rather than per workstation.

## Prompt injection

The realistic threat with a file-connected assistant is not that the model turns
malicious — it is that a document, an e-mail or a web page it reads contains text
that instructs it to do something the user never asked for.

Kobel's answer is that the instruction and the permission are separate. Text
inside a file cannot raise its own level. A file marked *Read only* stays read-only
however convincingly a document argues otherwise, a *Blocked* file is not
reachable by the tool layer, and anything the assistant did attempt is in the log.

Longer write-up: [docs/SECURITY.md](docs/SECURITY.md)

## Install

- **Windows** — [Microsoft Store](https://apps.microsoft.com/detail/xpfm44jjv5v00k)
  or a direct signed installer from <https://kobel.app>
- **macOS** — [Mac App Store](https://apps.apple.com/app/id6763767893)

Built with Tauri v2 (Rust core, web front end). The interface is available in
15 languages.

## Pricing

| | |
|---|---|
| **Basis** | Free — 1 AI, 2 folders / 50 files, 500 MB |
| **Pro** | €49 one-time |
| **Pro** | €3.90 / month |
| **Team** | Several sizes, see the website |

## Security contact

Vulnerability reports: see <https://kobel.app/.well-known/security.txt>
(PGP key published, ed25519). A CycloneDX SBOM is maintained for both the Rust
and the JavaScript components.

## Questions

Common questions — how to give ChatGPT access to local files, whether it is safe
to let an assistant read your disk, how this differs from pointing a filesystem
MCP server at a folder — are answered in [docs/FAQ.md](docs/FAQ.md).

## Links

- Website — <https://kobel.app/en/>
- Help — <https://kobel.app/hilfe.html>
- Connecting ChatGPT to local files — <https://kobel.app/chatgpt-local-files.html>
- Security — <https://kobel.app/security.html>

## License

The documentation in this repository is published under
[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). The Kobel application
itself is proprietary.
