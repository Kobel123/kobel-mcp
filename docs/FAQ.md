# Frequently asked questions

## How do I give ChatGPT access to my files on my own computer?

ChatGPT's chat window can reach a Model Context Protocol server, but it connects
from OpenAI's cloud rather than from your machine — so a `localhost` address will
not work. You need an MCP server that is reachable over HTTPS. Kobel does this by
opening a tunnel and handing ChatGPT that address, then filtering every request
against the permission level you gave each file. The step-by-step version is in
[CONNECT.md](CONNECT.md).

## Can Claude read files on my computer?

Claude Desktop can, through a local MCP server. Out of the box that means giving
a filesystem server one or more directory paths. Kobel replaces the paths with
per-file levels set in a graphical interface, so a folder can be editable while
one subfolder inside it stays invisible.

## Is it safe to let an AI assistant read my files?

It depends entirely on what "read my files" means in your setup. The risk that
matters is not the model deciding to misbehave — it is a document, e-mail or web
page it reads containing instructions aimed at the assistant. That is prompt
injection, and no amount of instruction-following training fully prevents it.

The defence that works is a boundary the text cannot argue with: a layer outside
the model that only takes instructions from a human. [SECURITY.md](SECURITY.md)
goes through this in detail, including what a permission layer does *not* solve.

## What is MCP?

The Model Context Protocol is an open standard for connecting AI assistants to
tools and data. An MCP server offers a set of named operations; a client (Claude
Desktop, ChatGPT, Cursor, Copilot, and others) calls them. Kobel is an MCP server
whose operations happen to be file operations, gated by permissions.

## Do I need to be a developer?

No. That is the point of it. The usual way to connect an assistant to files is to
edit a JSON configuration file by hand. Kobel writes those entries for you and the
permissions are set by clicking on folders.

## Does Kobel upload my files anywhere?

No. The server runs on your machine and reads files from your disk. The one place
data leaves your computer is the same place it always does: the content the
assistant receives goes to whichever AI provider you connected, because that is
what you asked it to do. There is no Kobel cloud in between.

## Why does ChatGPT need a tunnel when Claude Desktop does not?

Claude Desktop runs on your machine and starts the MCP server itself, so a local
connection is enough. ChatGPT's chat window is served from OpenAI's infrastructure
and connects outward from there, so it needs an address reachable from the public
internet. This is an architectural difference between the two clients, not a
shortcoming of either.

## Can I connect several AI assistants at once?

Yes. Each connected assistant is subject to the same file permissions, and the
activity log records which one made which call. The free tier is limited to one
assistant; the paid tiers are not.

## What happens if I withdraw a permission in the middle of a conversation?

The new level applies to the next tool call. Nothing needs restarting, and the
assistant simply gets a refusal where it previously got a file.

## How is this different from pointing a filesystem MCP server at a folder?

Three things: permissions are per file rather than per directory; there are five
levels rather than read/write; and everything is set in an interface rather than a
config file. The practical difference shows up in the common case — a project
folder with one subfolder full of credentials or client data. With directory-level
tools you either expose it or split the project.

## Is Kobel open source?

No. Kobel is a commercial, closed-source application. This repository is its public
documentation. That distinction is stated plainly in
[SECURITY.md](SECURITY.md#what-this-does-not-solve): a local-only claim from
closed-source software rests on trust, an SBOM and observable network behaviour —
not on an audit.

## Does it work offline?

The local part does: a client on the same machine can talk to Kobel with no
internet connection. Anything involving a cloud assistant obviously needs a
connection, as does the ChatGPT tunnel.

## Which languages does the interface speak?

15, including right-to-left layout for Arabic.

## Can a company roll this out centrally?

Yes. An administrator can publish a policy over a URL or a UNC path; clients read
it and apply it, so folder rules and blocked paths do not have to be set on each
workstation separately.

## What does it cost?

There is a free tier (one assistant, two folders or 50 files, 500 MB). Paid tiers
are €49 once or €3.90 per month, plus team tiers. Current details are on
<https://kobel.app/en/>.

---

Back to the [README](../README.md).
