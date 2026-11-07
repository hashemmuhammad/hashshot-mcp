# HashShot MCP server

Give your AI agent a camera on your Mac. The HashShot MCP server lets Claude, Claude Code, ChatGPT Codex, Cursor, VS Code, Windsurf and any other MCP client take screenshots, scroll and capture whole pages, read text on screen, record your screen and edit the result, with your permission each time.

The server ships inside [HashShot](https://hashshot.app), a screenshot and screen recording app for Mac made by [HashCode FZE](https://hashcode.ae). Everything runs locally on your Mac: there is no cloud and no account.

This repository holds the setup guide, example configurations and the [MCP Registry](https://registry.modelcontextprotocol.io/v0/servers?search=hashshot) entry. The app itself is not open source and its code is not here.

* Website: https://hashshot.app/mcp-screenshot-server
* MCP Registry name: `app.hashshot/mcp`
* Transport: stdio, local only
* Requirements: macOS 14 Sonoma or later, Apple silicon or Intel, and the HashShot app

## Install

1. Download HashShot from https://hashshot.app and move it to Applications. Every feature is free for 7 days; after that it is $25 once.
2. Open HashShot once and allow Screen Recording when macOS asks.
3. In the HashShot window, open **AI Agents** and click **Connect** next to your AI app. HashShot adds itself to that app's MCP settings.
4. Start a new chat or session in your AI app and ask for a screenshot.

## Manual setup

Every client points at the helper inside the app:

```
/Applications/HashShot.app/Contents/MacOS/hashshot-mcp
```

The helper starts HashShot in the background if it is not running, and passes MCP messages to it over a private local socket. Ready to copy files are in [examples](examples).

**Claude Code**

```bash
claude mcp add --scope user hashshot -- /Applications/HashShot.app/Contents/MacOS/hashshot-mcp
```

**Claude desktop app** (`~/Library/Application Support/Claude/claude_desktop_config.json`), **Cursor** (`~/.cursor/mcp.json`) and **Windsurf** (`~/.codeium/windsurf/mcp_config.json`)

```json
{
  "mcpServers": {
    "hashshot": {
      "command": "/Applications/HashShot.app/Contents/MacOS/hashshot-mcp",
      "args": []
    }
  }
}
```

**VS Code** (`~/Library/Application Support/Code/User/mcp.json`)

```json
{
  "servers": {
    "hashshot": {
      "type": "stdio",
      "command": "/Applications/HashShot.app/Contents/MacOS/hashshot-mcp",
      "args": []
    }
  }
}
```

**Codex** (`~/.codex/config.toml`)

```toml
[mcp_servers.hashshot]
command = "/Applications/HashShot.app/Contents/MacOS/hashshot-mcp"
args = []
```

Keep any servers already in those files, and restart the app after editing.

## Tools

| Group | Tool | What it does |
|---|---|---|
| Capture | `take_screenshot` | Capture a window, an area or the whole screen |
| | `scrolling_capture` | Scroll and capture a whole page or chat as one image |
| | `extract_text` | Read text on screen or in an image (OCR) |
| | `list_windows` | List windows to capture |
| | `arrange_window` | Resize or move a window before capturing |
| | `set_desktop_icons` | Hide or show desktop icons |
| Recording | `record_screen` | Record a short clip |
| | `start_recording`, `stop_recording` | Record for as long as needed |
| | `control_recording` | Pause, resume or cancel a recording |
| | `edit_video` | Trim, resize, zoom, add a background, reframe as vertical or square, burn in captions, export MP4 or GIF |
| | `video_info` | Inspect a recording and see frames from it |
| | `transcribe_recording` | Turn speech into captions, on the Mac |
| Images | `annotate_image` | Add arrows, shapes, text, steps and blur |
| | `redact_sensitive_data` | Find and blur emails, phone numbers, card numbers and API keys |
| | `remove_background` | Cut out the subject of an image |
| | `beautify_image` | Add a background, padding, corners and a shadow |
| | `combine_images` | Put images side by side or on top of each other |
| | `export_captures` | Export captures to files |
| Library | `search_history` | Find earlier captures, including by the words in them |
| | `get_capture`, `manage_capture` | Open, keep, rename or delete a capture |
| | `copy_to_clipboard` | Copy a capture |
| | `pin_screenshot` | Float a screenshot on top of other windows |
| | `show_in_hashshot` | Show something to the user in HashShot |
| Status | `get_status`, `list_styles` | Check permissions and available options |
| | `get_settings`, `update_settings` | Read or change HashShot settings |

You never call these by name. Ask for what you want and the agent picks the tool.

## Things to ask

* Take a screenshot of the Simulator window and tell me why the button is cut off.
* Capture the whole pricing page in Safari with a scrolling capture and check the spacing.
* Read the error in the terminal window and explain it.
* Record my screen for 20 seconds while I reproduce the bug, then make a GIF for the pull request.
* Blur the emails in the last screenshot and copy it to the clipboard.

Guides for [Claude Code](https://hashshot.app/claude-code-screenshots) and [Cursor](https://hashshot.app/cursor-screenshot-mcp).

## Privacy and permission

* HashShot asks before an agent captures or records. You choose whether to approve once or every time, and you can disconnect an app at any time in AI Agents.
* An agent only gets a capture when it asks for one. It never sees your screen in the background.
* Captures are saved on your Mac. The images or text an agent asks for are sent to the AI model you use, under that provider's own terms, the same as a screenshot you paste yourself.
* HashShot has no cloud and no account. See the [privacy policy](https://hashshot.app/privacy).

## Support

Email support@hashshot.app, or use Support inside the app. Issues in this repository are welcome for setup questions about the MCP server.

## License

The documentation and example files in this repository are under the [MIT License](LICENSE). The HashShot app is proprietary software by [HashCode FZE](https://hashcode.ae) and is not covered by that license.

Claude and Claude Code are trademarks of Anthropic. ChatGPT and Codex are trademarks of OpenAI. Cursor, VS Code, Windsurf, Mac and macOS are trademarks of their owners. HashShot is not affiliated with or endorsed by them.
