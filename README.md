# moe - a terminal text editor

**moe** is a lightweight, fast terminal text editor written in Go. It runs on Linux and macOS, adapts to the terminal window size, supports split-screen editing, syntax highlighting, and a built-in file browser.

Copyright 2026 by moshix. All rights reserved.

## Features

- Full terminal-based editing with responsive, fluid typing and scrolling
- Automatic terminal resize handling
- Syntax highlighting for Go, C, Python, Bash, and JavaScript
- Vertical split-screen with two independent buffers
- Built-in file browser overlay for navigating and opening files
- Command mode for search, save-as, and help
- Case-sensitive and case-insensitive search with wrap-around
- Dark theme (with provision for additional themes)
- Cross-platform: Linux (amd64, arm64), macOS (arm64), Windows (amd64)
- Static binaries with no runtime dependencies

## Installation

### Build from source

Requires Go 1.22 or later.

```bash
# Simple build for your current platform
go build -o moe .

# Or cross-compile all platforms
bash scripts/build.sh
ls bin/
```

The `scripts/build.sh` script produces static binaries in the `bin/` directory with the version number in the filename (e.g. `moe-0.1.0-linux-amd64`). The version is read automatically from `main.go`.

### Pre-built binaries

Download the appropriate binary from the `bin/` directory:

| Platform | Binary |
|---|---|
| Linux x86_64 | `moe-<version>-linux-amd64` |
| Linux ARM64 | `moe-<version>-linux-arm64` |
| macOS Apple Silicon | `moe-<version>-darwin-arm64` |
| Windows x86_64 | `moe-<version>-windows-amd64.exe` |

## Usage

```
moe                         Open with an empty buffer
moe file.txt                Open file.txt for editing
moe file1.txt file2.txt     Open in split-screen with both files
moe --version               Show version and build date
moe --help                  Show usage information
```

## Command Reference

moe has two ways to issue commands:

1. **Direct commands** -- press `Ctrl` + a key for immediate action
2. **Command mode** -- press `Ctrl-B` to open a command input line at the bottom of the screen (the status bar bumps up one row to make room), type a command, and press Enter

### Direct Commands (Ctrl + key)

| Key Combination | Action |
|---|---|
| `Ctrl-Q` | Quit the editor. If the file has unsaved changes, moe prompts "File changed. Save? (y/n/c)" in the status bar. Press `y` to save and quit, `n` to quit without saving, or `c` to cancel. |
| `Ctrl-S` | Save the current file. If no filename was given (new file), moe prompts "Save as:" in the status bar where you type the filename and press Enter. |
| `Ctrl-F` | Open the file browser overlay. Navigate the local directory tree to select a file to open in the current buffer. |
| `Ctrl-V` | Split the screen vertically into two panes. |
| `Ctrl-W` | Close the active pane. If the buffer has unsaved changes, prompts "Pane changed. Save? (y/n/c)" in the status bar. In single-pane mode, behaves like `Ctrl-Q`. |
| `Ctrl-P` then `Left` | Switch to the left pane (split mode only). Press `Ctrl-P`, then immediately press the Left arrow key. |
| `Ctrl-P` then `Right` | Switch to the right pane (split mode only). Press `Ctrl-P`, then immediately press the Right arrow key. |
| `Ctrl-N` | Scroll half a page down. |
| `Ctrl-U` | Scroll half a page up. |
| `Ctrl-T` | Go to the top of the file (first line). |
| `Ctrl-Z` | Go to the end of the file (last line). |
| `Ctrl-E` then `V` | Enter visual line select mode. The current line is highlighted. Use Up/Down arrows to extend the selection. Press Enter (or `y`) to copy the selected lines to the clipboard. Press `U` to UPPERCASE selected lines. Press `u` to lowercase selected lines. Press Escape to cancel. |
| `Ctrl-E` then `b` | Paste the clipboard contents **below** the current cursor line. |
| `Ctrl-E` then `a` | Paste the clipboard contents **above** the current cursor line. |
| `Ctrl-G` then `s` | Jump to the **start** of the current function. Works with Go, C, Python, Bash, and JavaScript. |
| `Ctrl-G` then `e` | Jump to the **end** of the current function. Works with Go, C, Python, Bash, and JavaScript. |
| `Ctrl-B` | Enter command mode. The status bar bumps up one row and a `:` command input line appears at the very bottom of the screen. |
| `Ctrl-X` | Find the next occurrence of the current search pattern. Wraps around to the top of the file. |

### Navigation Keys

| Key | Action |
|---|---|
| Arrow Up / Down / Left / Right | Move cursor one position in the given direction. Left at the beginning of a line moves to the end of the previous line. Right at the end of a line moves to the beginning of the next line. |
| Home | Move cursor to the beginning of the current line. |
| End | Move cursor to the end of the current line. |
| Page Up | Scroll one full page up. |
| Page Down | Scroll one full page down. |

### Quick Navigation (Escape prefix)

Press `Escape` followed by one of these keys for instant cursor repositioning within the current pane (no scrolling for T/B):

| Key sequence | Action |
|---|---|
| `Esc` then `0` | Move cursor to the **beginning** of the current line (column 0). |
| `Esc` then `$` or `z` | Move cursor to the **last character** of the current line. |
| `Esc` then `T` | Move cursor to the **first line** visible on screen (top of viewport), without scrolling. |
| `Esc` then `B` | Move cursor to the **last line** visible on screen (bottom of viewport), without scrolling. |
| `Esc` then `H` | Delete from the cursor position to the **beginning of the line**. |
| `Esc` then `J` | Delete from the cursor position to the **end of the line**. |
| `Esc` then `K` | **Delete** the current line. |
| `Esc` then `o` | Insert a new empty line **below** the current line and move the cursor to it. |
| `Esc` then `a` | Insert a new empty line **above** the current line and move the cursor to it. |
| `Esc` then `N` | **Split line** at cursor position. Text after the cursor moves to a new line below. |
| `Esc` then `U` | **Join line** with previous. Appends current line to the end of the line above (opposite of `Esc N`). |
| `Esc` then `M` then `1-9` | **Set mark** at the current cursor position. Marks are persistent across sessions. |
| `Esc` then `R` then `1-9` | **Recall mark**, jump cursor to the saved position. |
| `Esc` then `.` | **Repeat** the previous Escape action at the current cursor position. |

#### Examples

```
Esc 0       Jump to column 0 of the current line.
Esc $ or z  Jump to the last character of the current line.
Esc T       Jump to the top-most visible line on screen.
Esc B       Jump to the bottom-most visible line on screen.
Esc o       Insert a new empty line below the current line.
```

### Editing Keys

| Key | Action |
|---|---|
| Any printable character | Insert at cursor position. |
| Enter | Insert a new line (split the current line at the cursor). |
| Tab | Insert 4 spaces at the cursor position. |
| Backspace | Delete the character before the cursor. At the beginning of a line, joins with the previous line. |
| Delete | Delete the character at the cursor. At the end of a line, joins with the next line. |

### Copy & Paste (Ctrl-E prefix)

moe supports visual line selection for copying multiple lines and pasting them anywhere, including across panes.

1. Press `Ctrl-E` then `V` (capital V) to enter **visual line select** mode. The current line is highlighted.
2. Move Up/Down with the arrow keys (or Page Up/Down) to extend the selection. The status bar shows the number of selected lines.
3. Press **Enter** (or `y`) to copy the selected lines to the clipboard. The status bar shows "copyN lines".
4. Or press **U** to convert the selected lines to UPPERCASE, or **u** to convert to lowercase. The selection exits after the transformation.
5. Navigate to any position in any pane, then press `Ctrl-E` then `b` to paste **below** the current line, or `Ctrl-E` then `a` to paste **above** the current line.
6. Press **Escape** at any time during visual selection to cancel without copying.

The clipboard is shared across both panes in split mode.

#### Examples: Copy & Paste with Ctrl-E

**Copy 5 lines and paste them elsewhere:**

```
1. Position cursor on the first line you want to copy.
2. Press Ctrl-E, then V        → current line is highlighted
3. Press Down arrow 4 times    → 5 lines are now highlighted
4. Press Enter                 → status bar shows "copy5 lines"
5. Move cursor to the target location.
6. Press Ctrl-E, then b        → 5 lines are pasted below the cursor
```

**Copy lines from one pane and paste into the other:**

```
1. In the left pane, press Ctrl-E, then V
2. Select lines with Up/Down, press Enter to copy
3. Press Ctrl-P, then Right arrow  → switch to right pane
4. Navigate to the desired line.
5. Press Ctrl-E, then a            → lines are pasted above cursor
```

**Uppercase a block of code:**

```
1. Position cursor on the first line.
2. Press Ctrl-E, then V        → visual mode active
3. Select lines with Down arrow
4. Press U                     → all selected lines become UPPERCASE
```

**Lowercase a block of code:**

```
1. Press Ctrl-E, then V        → visual mode active
2. Select lines with Down arrow
3. Press u                     → all selected lines become lowercase
```

### Function Navigation (Ctrl-G prefix)

Press `Ctrl-G` followed by `s` or `e` to jump to the start or end of the current function.

| Key Sequence | Action |
|---|---|
| `Ctrl-G` then `s` | Jump to the first line of the enclosing function definition. |
| `Ctrl-G` then `e` | Jump to the last line of the enclosing function body. |

Language-specific detection:

| Language | How functions are detected |
|---|---|
| **Go** | `func` keyword at the start of a line; matched `{`...`}` braces. |
| **C** | Top-level lines containing `(` that are not control-flow keywords; matched braces. |
| **JavaScript** | `function` keyword, or `const`/`let`/`var` with `=>`; matched braces. |
| **Bash** | `function name` or `name()` patterns; matched braces. |
| **Python** | `def` / `async def` keyword; body determined by indentation level. |

The status bar shows the target line number after the jump. If the cursor is not inside a function, the status bar shows "No function found".

#### Examples: Function Navigation with Ctrl-G

**Jump to the start of the current Go function:**

```
1. You are editing main.go, cursor is somewhere inside a function.
2. Press Ctrl-G, then s         → cursor jumps to the "func ..." line
3. Status bar shows "Function start: line 42"
```

**Jump to the closing brace of a C function:**

```
1. You are editing utils.c, cursor is inside a function body.
2. Press Ctrl-G, then e         → cursor jumps to the closing "}"
3. Status bar shows "Function end: line 87"
```

**Navigate a Python function:**

```
1. You are editing app.py, cursor is inside a "def process():" block.
2. Press Ctrl-G, then s         → cursor jumps to the "def process():" line
3. Press Ctrl-G, then e         → cursor jumps to the last indented line
                                  of that function body
```

### Command Mode (Ctrl-B)

Press `Ctrl-B` to open the command input line. The status bar bumps up one row and a `:` prompt appears on the very bottom row of the screen, visually indicating that you are in command mode. Type one of the following commands and press Enter:

| Command | Action |
|---|---|
| `/pattern` | Search forward for "pattern" (case-sensitive). The cursor moves to the first match. Use `Ctrl-X` to find subsequent matches. |
| `\pattern` | Search forward for "pattern" (case-insensitive). The cursor moves to the first match. Use `Ctrl-X` to find subsequent matches. |
| `save/filename` | Save the current buffer to the given filename. The buffer's filename is updated to the new name. Example: `save/backup.txt` |
| `!command` | Run a local shell command and show its output in an overlay window. Use arrow keys to scroll, Escape to close. Example: `!ls -la` |
| `insert !command` | Run a local shell command and insert its output into the current buffer at the cursor position. Example: `insert !date` |
| `grep pattern files` | Run grep and show results in an overlay window. Supports standard grep flags. Examples: `grep TODO *.go`, `grep -rni error *`. Use arrow keys to navigate results, Enter to open the file at that match, Escape to close. |
| `%pattern` | Fuzzy search in the current buffer. Matches lines where all characters of "pattern" appear in order (not necessarily contiguous). Jumps directly to the first matching line, just like `/` and `\`. Use `Ctrl-X` to cycle to the next match (wraps around). |
| `replace X Y confirm` | Find all occurrences of X and confirm each replacement with Y. Press `y` to replace, `n` to skip, `Esc` to stop. The current match is highlighted. Status bar shows the running count. |
| `replace X Y ALL` | Replace all occurrences of X with Y without confirmation. `ALL` must be uppercase. Status bar shows total replaced. |
| `ai` | Open the AI assistant overlay. Requires `set ai-apikey=sk-...` in config. Type a query about the current file, Enter to send to ChatGPT (gpt-4o). Response is scrollable. Press `a` to apply code blocks to the buffer. |
| `ln` | Show line numbers in the left gutter of the current pane. Also accepts `linenumbers` or `line`. |
| `ln off` | Hide line numbers. Also accepts `linenumbers off` or `line off`. |
| `themes` | Open the theme picker overlay. Use arrow keys to select a theme, Enter to apply, Escape to cancel. |
| `help` | Display the built-in help page with all commands. Press Enter or Escape to close the help page. |
| Escape | Cancel command mode and return to normal editing. |
| Tab | Command completion. If one match, auto-completes. If multiple, shows a popup list growing upward. Use Arrow Up/Down to navigate, Enter or Tab to accept, Escape to dismiss. Typing further filters the list. |

#### Examples: Command Mode with Ctrl-B

**Search for a pattern (case-sensitive):**

```
1. Press Ctrl-B                → status bar bumps up, ":" prompt appears
2. Type /TODO                  → press Enter
3. Cursor jumps to first "TODO" match.
4. Press Ctrl-X                → jumps to the next match
5. Press Ctrl-X again          → wraps around to the top if needed
```

**Search for a pattern (case-insensitive):**

```
1. Press Ctrl-B
2. Type \error                 → press Enter
3. Matches "error", "Error", "ERROR", etc.
```

**Save the current file with a new name:**

```
1. Press Ctrl-B
2. Type save/backup/myfile.txt → press Enter
3. Status bar shows "Saved: backup/myfile.txt"
```

**Grep across your project files:**

```
1. Press Ctrl-B
2. Type grep -rni handleKey *.go  → press Enter
3. Overlay shows all matches with filenames and line numbers.
4. Use Up/Down to browse results, Enter to open the file at that line.
5. Press Escape to close the grep overlay.
```

**Toggle line numbers on:**

```
1. Press Ctrl-B
2. Type ln                     → press Enter
3. Line numbers appear in the left gutter.
4. To turn off: Ctrl-B, type ln off, press Enter.
```

**Run a shell command and view its output:**

```
1. Press Ctrl-B
2. Type !ls -la                → press Enter
3. An overlay appears showing the directory listing.
4. Scroll with Up/Down or Page Up/Down, Escape to close.
```

**Run a shell command and insert its output into the buffer:**

```
1. Position cursor where you want the output inserted.
2. Press Ctrl-B
3. Type insert !date           → press Enter
4. The current date/time is inserted below the cursor line.
5. Status bar shows "Inserted 1 lines from: date"
```

**Insert the output of a complex command:**

```
1. Press Ctrl-B
2. Type insert !cat /etc/hostname   → press Enter
3. The contents of /etc/hostname are inserted at the cursor.
```

**Fuzzy search in the current buffer:**

```
1. Press Ctrl-B
2. Type %hndlkey               → press Enter
3. Cursor jumps to the first line containing "h", "n", "d", "l", "k", "e", "y"
   in that order (e.g. "func handleKey(ev ..." matches).
4. Press Ctrl-X to jump to the next fuzzy match (wraps around).
```

**Replace with confirmation:**

```
1. Press Ctrl-B
2. Type replace foo bar confirm    → press Enter
3. Cursor jumps to first "foo", highlighted.
4. Press y to replace with "bar", or n to skip.
5. Repeats for each occurrence. Press Esc to stop early.
6. Status bar shows "3 replaced" when done.
```

**Replace all without confirmation:**

```
1. Press Ctrl-B
2. Type replace foo bar ALL        → press Enter
3. All occurrences of "foo" are replaced with "bar" instantly.
4. Status bar shows "12 replaced".
```

**Ask the AI assistant about your code:**

```
1. Add 'set ai-apikey=sk-...' to ~/.config/moe/moe.cnf
2. Open a file in moe.
3. Press Ctrl-B
4. Type ai                    → press Enter
5. AI overlay opens with a prompt.
6. Type: "Add error handling to this function" → press Enter
7. AI response appears in a scrollable overlay.
8. Press 'a' to apply the code to your buffer, or Esc to close.
```

**Switch themes:**

```
1. Press Ctrl-B
2. Type themes                 → press Enter
3. A picker overlay appears with 6 themes.
4. Use Up/Down to preview, Enter to select, Escape to cancel.
```

### File Browser (Ctrl-F)

Press `Ctrl-F` to open the file browser as a centered overlay on the screen. The browser shows the contents of the current working directory.

| Key | Action |
|---|---|
| Arrow Up / Down | Move the selection highlight up or down. |
| Enter | Open the selected file into the current buffer, or navigate into the selected directory. |
| `..` entry | Navigate to the parent directory. |
| Page Up / Page Down | Scroll the file list by 10 entries. |
| Escape | Close the file browser without selecting a file. |

Directories are listed first (sorted alphabetically), then files (sorted alphabetically). Hidden files (names starting with `.`) are not shown.

## Split Screen

moe supports vertical split-screen editing with two independent buffers.

### Opening a split

- Press `Ctrl-V` to split the screen. A vertical separator line appears in the middle. The right pane starts with an empty buffer.
- Alternatively, invoke moe with two filenames: `moe file1.txt file2.txt` opens directly in split mode.

### Working in split mode

- Press `Ctrl-P` then `Left arrow` to switch to the left pane.
- Press `Ctrl-P` then `Right arrow` to switch to the right pane.
- The status bar at the bottom of the screen always belongs to the **active** pane. The inactive pane has no status bar.
- All editing commands (`Ctrl-S`, `Ctrl-F`, `Ctrl-B`, etc.) apply to the active pane.

### Closing a pane

- Press `Ctrl-W` to close the active pane. If the buffer has unsaved changes, moe prompts "Pane changed. Save? (y/n/c)".
- When the second pane is closed, the remaining pane takes the full screen.
- In single-pane mode, `Ctrl-W` behaves the same as `Ctrl-Q` (quit).

## Brace Scope Guides

When editing **Go** or **C** files (`.go`, `.c`, `.h`, `.cpp`, `.cc`, `.cxx`, `.hpp`), moe displays faint gray guide lines on the left edge of the editor. These guides show which `{` `}` brace pairs enclose the current cursor position:

- **Multiple nesting levels** are shown simultaneously -- deeper scopes appear slightly brighter.
- `┌` marks the line containing the opening `{`.
- `└` marks the line containing the closing `}`.
- `│` marks continuation lines between the braces.
- Guides update automatically as you move the cursor or edit the buffer.
- Up to 4 nesting levels are displayed to avoid consuming too much horizontal space.
- Results are cached per buffer edit version for performance.

```
┌ func main() {
│┌    if x > 0 {
││        doSomething()
│└    }
└ }
```

## Marks

moe supports persistent file marks (similar to vim marks). You can set up to 9 marks per file, and they are saved to disk so they survive editor restarts.

- Press `Esc M` then a digit `1`-`9` to set a mark at the current cursor position.
- Press `Esc R` then a digit `1`-`9` to recall the mark and jump back to the saved position.

Marks are stored under `~/.config/moe/marks/`, one file per source file. The mark file stores the line and column (0-based) for each mark number.

```
Esc M 3       Set mark 3 at current cursor (Ln 42, Col 15)
Esc R 3       Jump to mark 3 (Ln 42, Col 15)
```

If the file has been edited and the saved line no longer exists, the cursor is clamped to the last line.

## Status Bar

The status bar is always displayed at the bottom row of the screen in reverse video (light text on dark background swapped). It shows:

```
 filename [+] | Ln 42/100 Col 15
```

- **filename** -- the name of the file in the active buffer, or `[new file]` if no file is loaded.
- **[+]** -- appears when the buffer has unsaved changes.
- **Ln xx/nn** -- current line number / total lines.
- **Col cc** -- current column number.

The status bar is also used for prompts:
- "Save as: " when saving a new file (`Ctrl-S` with no filename)
- "File changed. Save? (y/n/c): " when quitting with unsaved changes (`Ctrl-Q`)

## Syntax Highlighting

moe automatically detects the programming language from the file extension and applies syntax highlighting. Supported languages:

| Language | Extensions |
|---|---|
| Go | `.go` |
| C | `.c`, `.h` |
| Python | `.py` |
| Bash | `.sh`, `.bash` |
| JavaScript | `.js` |

Files with unrecognized extensions are displayed as plain text.

The default dark theme uses a Catppuccin-inspired color palette with distinct colors for keywords, strings, numbers, comments, functions, operators, and more.

## Themes

moe ships with 4 built-in themes:

| Theme | Description |
|---|---|
| `dark` | Default dark theme with a Catppuccin-inspired color palette. |
| `solarized-dark` | Solarized Dark -- warm muted tones on a dark blue-green background. |
| `monokai` | Monokai -- vibrant colors on a near-black background, popular in Sublime Text. |
| `gruvbox` | Gruvbox Dark -- retro-groove palette with warm earthy tones. |

To switch themes at runtime, press `Ctrl-B` to enter command mode, then type `themes` and press Enter. A theme picker overlay appears where you can navigate with the arrow keys and press Enter to select a theme. The change takes effect immediately. An asterisk (`*`) marks the currently active theme.

The selected theme is automatically saved to `~/.config/moe/moe.cnf` and restored on next launch.

## Configuration

moe reads settings from `~/.config/moe/moe.cnf` on startup. The file uses a vimrc-style format:

```
" moe editor configuration
" format: set key=value

set theme=monokai
set city=Milan
set statusbar=time,weather
```

### Supported settings

| Setting | Values | Default | Description |
|---|---|---|---|
| `theme` | `dark`, `solarized-dark`, `monokai`, `gruvbox` | `dark` | Color theme |
| `linenumbers` | `on`, `off` | `off` | Show line numbers in the gutter |
| `city` | Any city name (e.g. `Milan`, `New York`, `Tokyo`) | *(none)* | City used for weather lookups via [wttr.in](https://wttr.in) |
| `statusbar` | Comma-separated: `time`, `weather` | *(none)* | Extra information to show on the right side of the status bar |
| `ai-apikey` | OpenAI API key (e.g. `sk-...`) | *(none)* | API key for the AI assistant (`Ctrl-B ai`). Uses the gpt-4o model. |

#### Status bar: time and weather

When `set statusbar=time` is configured, the local computer time (HH:MM) is shown on the right side of the status bar, updating every second.

When `set statusbar=weather` is configured along with `set city=Milan`, moe will:

1. Wait 30 seconds after startup (to avoid slowing down the editor launch).
2. Check for internet connectivity.
3. If online, fetch the current temperature and weather from [wttr.in](https://wttr.in) for the configured city.
4. Display the weather (e.g. `☀️ +22°C`) on the right side of the status bar.

You can combine both: `set statusbar=time,weather` shows both time and weather separated by `|`.

If there is no internet connection, the weather portion is silently omitted.

Lines starting with `"` are comments. The `set` keyword is optional (`theme=dark` also works). The file is created automatically when you first change a setting (e.g. via the theme picker).

## Project Structure

```
moe/
  main.go           Entry point, argument parsing, version
  editor.go         Core editor, main event loop
  buffer.go         Text buffer, cursor, editing operations, file I/O
  screen.go         Rendering engine, viewport management
  highlight.go      Syntax highlighting with chroma
  theme.go          Theme definitions and color mappings
  statusbar.go      Status bar rendering and prompts
  keys.go           Key binding registry
  command.go        Command mode input and dispatch
  split.go          Split-screen pane management
  filebrowser.go    File browser overlay
  search.go         Search state and pattern matching
  help.go           Help page content and rendering
  config.go         Configuration file (~/.config/moe/moe.cnf) management
  funcnav.go        Function-boundary navigation (Go, C, Python, Bash, JS)
  themepicker.go    Theme selection overlay
  grepview.go       Grep results overlay
  statusinfo.go     Background time/weather updates for status bar
  runner.go         Idle surfer animation state machine
  bracescope.go     Brace scope guide computation and caching
  aiview.go         AI assistant overlay (ChatGPT integration)
  marks.go          Persistent file marks (Esc M/R)
  scripts/
    build.sh        Cross-compilation build script
  bin/              Build output directory
```

## Building

### Quick build (current platform)

```bash
go build -o moe .
```

### Release build (all platforms)

```bash
bash scripts/build.sh
```

This produces statically linked binaries in `bin/` with names like `moe-0.1.0-linux-amd64`. The version is extracted from the `Version` variable in `main.go`.

## Version

Check the current version:

```bash
./moe --version
```

To change the version, edit the `Version` variable in `main.go`:

```go
var Version = "0.1.0"
```

## License

All rights reserved. Copyright 2026 by moshix.
