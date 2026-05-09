# Deep Link Matrix

Last updated: 2026-02-10
Scope: practical deep links for agent workflows, especially Slack handoff links.

## Support Levels

- `Supported`: Officially documented and/or repeatedly verified.
- `Partial`: Works for some routes, but route coverage is incomplete.
- `Unknown`: Scheme exists, but route behavior is not clearly documented.
- `Not available`: No known official deep-link format.

## Matrix

| App | Scheme | Support | Known Link Patterns | Notes |
| --- | --- | --- | --- | --- |
| Codex Desktop | `codex://` | Supported | `codex://threads/<thread-uuid>`, `codex://threads/new`, `codex://settings` | Good for thread handoff links. |
| Cursor | `cursor://` | Supported | `cursor://file/<absolute-path>:<line>:<column>`, `cursor://file/<absolute-folder-path>/` | Works well for file/folder open links. |
| VS Code | `vscode://` | Supported | `vscode://file/<absolute-path>:<line>:<column>`, `vscode://file/<absolute-folder-path>/` | Requires VS Code installed on clicker machine. |
| VS Code Insiders | `vscode-insiders://` | Supported | `vscode-insiders://file/<absolute-path>:<line>:<column>` | Insiders-specific scheme. |
| Visual Studio (Windows IDE) | n/a | Not available | n/a | Use CLI fallback (`devenv /edit <path>`). |
| Claude Desktop | `claude://` | Unknown | `claude://...` | Scheme registration exists, but stable public route list is unclear. |
| Xcode | `xcode://` | Partial | `xcode://...` | Scheme exists; file-open route details are not well documented. |
| Windsurf | `windsurf://` | Supported | `windsurf://file/<absolute-path>:<line>:<column>`, `windsurf://file/<absolute-folder-path>/` | Same pattern as Cursor/VS Code; verified working on macOS. |
| Zed | `zed://` | Partial | `zed://file/<absolute-path>:<line>` | Personal note: column number is not supported in Zed deep links as of early 2026. |
| JetBrains IDEs | `jetbrains://` | Partial | `jetbrains://<product>/navigate/reference?project=<name>&path=<relative-path>` | Personal note: tested with IntelliJ IDEA; product slug varies (e.g. `idea`, `pycharm`). Requires Toolbox App running. |
| CLI-only agents | n/a | Not available | n/a | No standard clickable deep-link protocol without custom handlers. |

## Slack Format

Slack clickable links must be formatted as:

```text
<url|label>
```

Examples:

- `<codex://threads/123e4567-e89b-12d3-a456-426614174000|Open in Codex>`
- `<cursor://file/<absolute-path>:<line>:<column>|Open in Cursor>`
- `<vscode://file/<absolute-path>:<line>:<column>|Open in VS Code>`
- `<windsurf://file/<absolute-path>:<line>:<column>|Open in Windsurf>`
- `<zed://file/<absolute-path>:<line>|Open in Zed>` <!-- no column support -->
- `<jetbrains://idea/navigate/reference?project=<name>&path=<relative-path>|Open in IntelliJ>` <!-- requires Toolbox -->

## Verification Commands

Check app schemes:

```bash
/usr/libexec/PlistBuddy -c 'Print :CFBundleURLTypes' /Applications/<App>.app/Contents/Info.plist
```

Smoke test a deep link:

```bash
open '<scheme>://...'
```

## Fallback Patterns

When no deep link exists:

1. Provide the file path and app command (`devenv /edit`, `code`, etc.).
2. State that no official deep-link format is known.
3. Offer a supported alternative app link when possible.

## Sources

- Cursor deeplinks docs: https://cursor.com/docs/deeplinks
- VS Code URL docs: https://code.visualstudio.com/docs
- JetBrains Toolbox deep links: https://www.jetbrains.com/help/idea/working-with-the-ide-features-from-command-line.html
