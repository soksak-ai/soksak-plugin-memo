# Memo (soksak-plugin-memo)

A per-project memo panel. Opens a notepad (✎) in the right sidebar and auto-saves on
input.

- **Per-project isolation**: The storage key is derived from the project root path
  (unsafe characters replaced with `_` + hash suffix). Different project roots use
  different keys, so memos never mix. Contexts without a root share a single
  `memo.global` key.
- **Persists across restarts**: File-based storage in a dedicated directory
  (`~/.soksak/plugins-data/soksak-plugin-memo/`) — survives app restarts.
- **Auto-save**: Saves with a 400ms debounce after input. Shows "Saving…/Saved" in the
  status bar; failures are surfaced honestly as red error text. Pending saves are
  flushed on panel unmount or plugin deactivation.

## Permission Rationale

| Permission | Reason |
| --- | --- |
| `ui` | Register and display a memo view (`panel`) in the sidebar |
| `storage` | Read/write memo text in the plugin's dedicated storage |

No other permissions are declared — no filesystem, command, or network access.

## Installation

```bash
# GitHub shorthand
sok plugin.install '{"source":"<user>/soksak-plugin-memo"}'

# Local path (this example directory)
sok plugin.install '{"source":"/path/to/repo/examples/plugins/soksak-plugin-memo"}'

# Activation requires consent from the app UI
sok plugin.enable '{"id":"soksak-plugin-memo"}'
```

## Usage

1. Open ✎ (Memo) from the icon rail in the right sidebar.
2. Type freely in the text area — it auto-saves after 400ms.
3. Switching projects loads that project's separate memo.

## DOM Exposure (structural addresses)

Elements accessible from outside (address click/measurement, E2E) are declared in
`contributes.nodes` and have `data-node` attributes on the actual elements. Undeclared
elements are inaccessible (explicit error). Node addresses take the form
`.../view/soksak-plugin-memo.panel/node/<data-node>`.

| Node | data-node | Description |
| --- | --- | --- |
| `input` | `input` | Memo input surface (textarea). Target for clicks and input |
| `status` | `status` | Save status bar. Target for reading "Saved/Save failed" text |

There are no save or delete buttons — input is auto-saved with a 400ms debounce and
there is no delete UI. Only these two existing elements are exposed.
