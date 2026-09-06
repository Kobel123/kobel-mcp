# Connecting an AI client

Kobel is an ordinary MCP server, so any MCP client can talk to it. What differs
between clients is *how* they reach it — and the important split is local versus
remote.

## Local versus remote — why ChatGPT is different

Most clients run on your machine and start the MCP server themselves. Kobel writes
the entry into their configuration file, they launch it, done.

ChatGPT's chat window does not work that way. **OpenAI connects to MCP servers from
its own cloud, not from your device.** A `localhost` address is therefore not
reachable for it — not as a bug, but by architecture. Kobel handles this by opening
an HTTPS tunnel and giving ChatGPT the public address of that tunnel instead.

This is the single most common source of confusion, so it is worth stating plainly:
for Claude Desktop, Cursor, Copilot and Codex the connection is local. For the
ChatGPT chat window it goes through a tunnel.

## Claude Desktop

1. In Kobel, open **Connect AI** and choose Claude Desktop.
2. Kobel writes the server entry into Claude's configuration.
3. **Quit Claude Desktop completely and start it again** — it reads the
   configuration only at launch. On Windows, closing the window is not enough;
   quit it from the tray icon, then open Claude Desktop again from the Start menu.
4. In Claude, ask it to call a Kobel tool explicitly, for example
   *"list my root folders using kobel_list_root_folders"*.

Do not ask an assistant *"do you have access to my files?"* — several clients have
file access of their own and will answer yes without ever touching Kobel. Always
have it call a `kobel_` tool by name.

## ChatGPT (the normal chat window)

1. In Kobel, start the ChatGPT assistant. It opens the tunnel and shows you the
   address.
2. In ChatGPT, open the **Plugins / Connectors** area in the sidebar and use
   **+** (top right).
3. Name it, choose **Server URL** as the connection type, paste the address.
4. **Authentication must be set to "None".** The default is OAuth, and OAuth will
   fail here.
5. Tick the confirmation box, click **Create**, then **Connect** in the next dialog.

Two things that reliably go wrong:

- **An existing entry with the same name blocks the new one.** ChatGPT does not
  allow the server URL of an existing entry to be changed — the "..." menu only
  offers rename, describe, disconnect and delete. Delete the old entry first, or
  give the new one a different name.
- **The tunnel address changes every time Kobel starts.** Since the URL of an
  existing entry cannot be edited, the entry has to be recreated after a restart.
  This is a limitation of ChatGPT's connector UI, not of the tunnel.

## OpenAI Codex

Codex has its own entry in Kobel, separate from ChatGPT, because it connects
locally rather than through the tunnel. Kobel writes the TOML block for you.

**Codex reads its configuration only at startup.** After adding the entry, quit
Codex completely and start it again — otherwise it will not see Kobel. Then have it
call a `kobel_` tool by name to confirm.

Newer builds also offer a **Plugins** area with Public / Personal tabs and an
**Add** button; a personal plugin can be created there from an MCP URL.

## Cursor, GitHub Copilot / VS Code, Gemini CLI

All three take a local MCP server entry. Choose the client in Kobel's **Connect
AI** list and follow the on-screen steps; Kobel writes the configuration and tells
you when a restart is required.

## Other clients

Kobel ships templates for Qwen Code, Kimi, CodeBuddy, Trae, Cherry Studio, Chatbox,
Roo Code, Warp and the ChatGPT desktop app. Anything else can be pointed at Kobel
manually — it is a standard MCP server.

## Checking that it worked

Ask for `kobel_list_root_folders`. You should get back the folders you shared, with
their permission levels. If you get an answer that lists your files but does *not*
name a `kobel_` tool, the client is using its own file access and Kobel is not in
the loop.

---

Full step-by-step help, in 15 languages: <https://kobel.app/hilfe.html> ·
Back to the [README](../README.md).
