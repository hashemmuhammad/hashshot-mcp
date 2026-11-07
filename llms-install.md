# Installing the HashShot MCP server (for AI agents)

These steps are for an AI agent setting up HashShot for its user. HashShot is a Mac app; its MCP server is a small helper inside the app, so there is nothing to build or install with a package manager.

## 1. Check the requirements

* macOS 14 Sonoma or later. Check with `sw_vers -productVersion`.
* The HashShot app in Applications. Check with:

```bash
test -x /Applications/HashShot.app/Contents/MacOS/hashshot-mcp && echo installed
```

If it is not installed, ask the user to download it from https://hashshot.app/download/latest, drag HashShot to Applications and open it once. Do not install it for them. Every feature is free for 7 days; after that it is a paid app ($25 once).

## 2. Ask the user to allow screen recording

The first time HashShot opens, macOS asks for Screen Recording permission. The user has to allow it in System Settings, Privacy & Security, Screen & System Audio Recording. An agent cannot do this step.

## 3. Add the server to the MCP settings

The server runs over stdio with no arguments and no environment variables:

```json
{
  "mcpServers": {
    "hashshot": {
      "command": "/Applications/HashShot.app/Contents/MacOS/hashshot-mcp",
      "args": [],
      "disabled": false
    }
  }
}
```

Merge the `hashshot` entry into the existing `mcpServers` object and keep every other server. For Cline in VS Code the file is `~/Library/Application Support/Code/User/globalStorage/saoudrizwan.claude-dev/settings/cline_mcp_settings.json`. Other clients are listed in the [README](README.md).

## 4. Check that it works

Call the `get_status` tool. It reports whether HashShot is running and which permissions are granted. The helper starts HashShot in the background if it is not running.

Then try `take_screenshot`. HashShot asks the user for permission before the first capture; wait for them to approve it.

## Notes

* Everything runs on the user's Mac. HashShot has no cloud and no account.
* No API key or token is needed.
* Support: support@hashshot.app
