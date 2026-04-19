# moe - a terminal text editor

**moe** is a lightweight, fast terminal text editor written in Go. It runs on Linux and macOS and Windoz, adapts to the terminal window size, supports split-screen editing, syntax highlighting, and a built-in file browser. It also integrates with LLMs so you can do LLM work on terminal-based editor while connect to remote machines. 

This is an editor developed the way my fingers and my thinking work. It's meant for fast editing of
Go, C, js, Java files.

## Features

- Full terminal-based editing with responsive, fluid typing and scrolling
- Automatic terminal resize handling
- Syntax highlighting for Go, C, Python, Bash, and JavaScript
- Vertical and horizontal split-screen with two independent buffers
- Built-in file browser overlay for navigating and opening files
- search and fuzzy search (when you don't remember the function names)
- Themes 
- AI integration with ChatGPT (requires API key)
- undo buffers
- braces scope visualization
- function-related search, replace etc. 
- Mouse support (click to position, drag to select, scroll wheel, pane switching)



## Usage

```
moe                         Open with an empty buffer
moe file.txt                Open file.txt for editing
moe file.txt +100           Open file.txt at line 100
moe file1.txt file2.txt     Open in split-screen with both files
moe -view file.txt          Open in read-only view mode
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
| `Ctrl-F` | Open the file browser overlay. Navigate the local directory tree to select a file to open in the current buffer. If the current buffer has unsaved changes, moe prompts "File changed. Save? (y/n/c)" before loading the new file. |
| `Ctrl-V` | Split the screen vertically into two panes. |
| `Ctrl-W` | Close the active pane. If the buffer has unsaved changes, prompts "Pane changed. Save? (y/n/c)" in the status bar. In single-pane mode, behaves like `Ctrl-Q`. |
| `Ctrl-P` then `Left` | Switch to the left pane (split mode only). Press `Ctrl-P`, then immediately press the Left arrow key. |
| `Ctrl-P` then `Right` | Switch to the right pane (split mode only). Press `Ctrl-P`, then immediately press the Right arrow key. |
| `Ctrl-N` | Scroll half a page down. |
| `Ctrl-U` | Scroll half a page up. |
| `Ctrl-1` | Jump to the first line of the buffer. |
| `Ctrl-0` | Jump to the last line of the buffer. |
| `Ctrl-E` then `V` | Enter visual line select mode. The current line is highlighted. Use Up/Down arrows to extend the selection. Press Enter (or `y`) to copy the selected lines to the clipboard. Press `U` to UPPERCASE selected lines. Press `u` to lowercase selected lines. Press `I` to enter multi-line prefix insert (type text + Enter to prepend to every selected line). Press `A` to enter multi-line suffix append (type text + Enter to append to every selected line). Press Escape to cancel. |
| `Ctrl-E` then `B` | Enter rectangular block select mode. Move cursor to form a block. Press Enter to copy, Ctrl-D to delete, Escape to cancel. Use Ctrl-E a/b to paste. |
| `Ctrl-E` then `b` | Paste the clipboard contents **below** the current cursor line. |
| `Ctrl-E` then `a` | Paste the clipboard contents **above** the current cursor line. |
| `Ctrl-G` then `s` | Jump to the **start** of the current function. Works with Go, C, Python, Bash, and JavaScript. |
| `Ctrl-G` then `e` | Jump to the **end** of the current function. Works with Go, C, Python, Bash, and JavaScript. |
| `Ctrl-B` | Enter command mode. The status bar bumps up one row and a `:` command input line appears at the very bottom of the screen. |
| `Ctrl-X` | Find the next occurrence of the current search pattern. Wraps around to the top of the file. |
| `Shift+Ctrl-X` | Find the previous occurrence of the current search pattern. Wraps around to the bottom of the file. |
| `Ctrl-D` | Delete the current line. |
| `Shift+Ctrl-D` | Delete the line above the current line. |
| `Ctrl-A` | Delete from cursor to end of line. |
| `Shift+Ctrl-A` | Delete from cursor to beginning of line. |
| `Ctrl-L` | Delete the current word under the cursor. |
| `Ctrl-O` | Delete the character under the cursor. |
| `Ctrl-K` | Start character-level visual select. Move cursor to extend selection, `Ctrl-D` to delete, Enter to copy, Escape to cancel. |
| `Ctrl-Y` | Undo the last edit action. Supports up to 20 levels of undo. |
| `Ctrl-R` | Redo the last undo action. |

### Navigation Keys

| Key | Action |
|---|---|
| Arrow Up / Down / Left / Right | Move cursor one position in the given direction. Left at the beginning of a line moves to the end of the previous line. Right at the end of a line moves to the beginning of the next line. |
| Home | Move cursor to the beginning of the current line. |
| End | Move cursor to the end of the current line. |
| Page Up | Scroll one full page up. |
| Page Down | Scroll one full page down. |

### Word & Line Navigation (Ctrl+Arrow)

| Key | Action |
|---|---|
| Ctrl+Right | Jump to the beginning of the next word |
| Ctrl+Left | Jump to the beginning of the previous word |
| Ctrl+Down | Jump to the first word of the next line |
| Ctrl+Up | Jump to the first word of the previous line |
| Shift+Ctrl+Right | Jump to the end of the current/next word |
| Shift+Ctrl+Left | Jump to the end of the previous word |
| Shift+Ctrl+Down | Jump to the last word of the next line |
| Shift+Ctrl+Up | Jump to the last word of the previous line |

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

1. Press `Ctrl-E` then `V` (capital V) to enter **visual line select** mode. The current line is highlighted. Or press `Ctrl-E` then `B` (capital B) to enter **rectangular block select** mode.
2. Move Up/Down (and Left/Right for block mode) with the arrow keys to extend the selection. The status bar shows the selection size.
3. Press **Enter** (or `y`) to copy the selected lines to the clipboard. The status bar shows "copyN lines".
4. Or press **U** to convert the selected lines to UPPERCASE, or **u** to convert to lowercase. The selection exits after the transformation.
5. Or press **I** to enter **multi-line prefix insert** mode — type any text and press Enter to prepend it to the start of every selected line.
6. Or press **A** to enter **multi-line suffix append** mode — type any text and press Enter to append it to the end of every selected line.
7. Navigate to any position in any pane, then press `Ctrl-E` then `b` to paste **below** the current line, or `Ctrl-E` then `a` to paste **above** the current line.
8. Press **Escape** at any time during visual selection (or multi-insert) to cancel.

The clipboard is shared across both panes in split mode.

#### Examples: Copy & Paste with Ctrl-E

**Copy 5 lines and paste them elsewhere:**

```
1. Position cursor on the first line you want to copy.
2. Press Ctrl-E, then V        -> current line is highlighted
3. Press Down arrow 4 times    -> 5 lines are now highlighted
4. Press Enter                 -> status bar shows "copy5 lines"
5. Move cursor to the target location.
6. Press Ctrl-E, then b        -> 5 lines are pasted below the cursor
```

**Copy lines from one pane and paste into the other:**

```
1. In the left pane, press Ctrl-E, then V
2. Select lines with Up/Down, press Enter to copy
3. Press Ctrl-P, then Right arrow  -> switch to right pane
4. Navigate to the desired line.
5. Press Ctrl-E, then a            -> lines are pasted above cursor
```

**Uppercase a block of code:**

```
1. Position cursor on the first line.
2. Press Ctrl-E, then V        -> visual mode active
3. Select lines with Down arrow
4. Press U                     -> all selected lines become UPPERCASE
```

**Lowercase a block of code:**

```
1. Press Ctrl-E, then V        → visual mode active
2. Select lines with Down arrow
3. Press u                     → all selected lines become lowercase
```

**Comment out 4 lines (prefix insert):**

```
1. Position cursor on the first line to comment.
2. Press Ctrl-E, then V        → visual mode active, line highlighted
3. Press Down arrow 3 times    → 4 lines highlighted
4. Press I                     → status bar: "Prefix insert (4 lines): |"
5. Type "// "                  → status bar: "Prefix insert (4 lines): // |"
6. Press Enter                 → "// " prepended to all 4 lines
```

**Add a trailing comment to several lines (suffix append):**

```
1. Press Ctrl-E, then V        → visual mode active
2. Select lines with Down arrow
3. Press A                     → status bar: "Suffix append (N lines): |"
4. Type "  // TODO"            → status bar: "Suffix append (N lines):   // TODO|"
5. Press Enter                 → "  // TODO" appended to every selected line
```

**Cancel mid-way:**

```
While typing in prefix/suffix mode, press Esc → "Multi-insert cancelled"
The buffer is unchanged.
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
2. Press Ctrl-G, then s         -> cursor jumps to the "func ..." line
3. Status bar shows "Function start: line 42"
```

**Jump to the closing brace of a C function:**

```
1. You are editing utils.c, cursor is inside a function body.
2. Press Ctrl-G, then e         -> cursor jumps to the closing "}"
3. Status bar shows "Function end: line 87"
```

**Navigate a Python function:**

```
1. You are editing app.py, cursor is inside a "def process():" block.
2. Press Ctrl-G, then s         -> cursor jumps to the "def process():" line
3. Press Ctrl-G, then e         -> cursor jumps to the last indented line
                                  of that function body
```

### Command Mode (Ctrl-B)

Press `Ctrl-B` to open the command input line. The status bar bumps up one row and a `:` prompt appears on the very bottom row of the screen, visually indicating that you are in command mode. Type one of the following commands and press Enter:

| Command | Action |
|---|---|
| `/pattern` | Search forward for "pattern" (case-sensitive). The cursor moves to the first match. Use `Ctrl-X` to find subsequent matches, `Shift+Ctrl-X` to find previous matches. |
| `\pattern` | Search forward for "pattern" (case-insensitive). The cursor moves to the first match. Use `Ctrl-X` to find subsequent matches, `Shift+Ctrl-X` to find previous matches. |
| `save/filename` | Save the current buffer to the given filename. The buffer's filename is updated to the new name. Example: `save/backup.txt` |
| `!command` | Run a local shell command and show its output in an overlay window. Use arrow keys to scroll, Escape to close. Example: `!ls -la` |
| `insert !command` | Run a local shell command and insert its output into the current buffer at the cursor position. Example: `insert !date` |
| `grep pattern files` | Run grep and show results in an overlay window. Supports standard grep flags. Examples: `grep TODO *.go`, `grep -rni error *`. Use arrow keys to navigate results, Enter to open the file at that match, Escape to close. |
| `%pattern` | Fuzzy search in the current buffer. Matches lines where all characters of "pattern" appear in order (not necessarily contiguous). Jumps directly to the first matching line, just like `/` and `\`. Use `Ctrl-X` to cycle to the next match, `Shift+Ctrl-X` for the previous match (wraps around). |
| `:N` | Jump to line N. Type `:nnn` and press Enter (e.g. `:123`) to move the cursor to that line. |
| `replace X Y confirm` | Find all occurrences of X and confirm each replacement with Y. Press `y` to replace, `n` to skip, `Esc` to stop. The current match is highlighted. Status bar shows the running count. |
| `replace X Y ALL` | Replace all occurrences of X with Y without confirmation. `ALL` must be uppercase. Status bar shows total replaced. |
| `delete-buffer` | Delete the entire contents of the active pane's buffer and place the cursor at the top of the screen. Only affects the current pane. The buffer is marked as modified. Undoable with `Ctrl-Y`. |
| `reset-virgin` | Reload the buffer from disk, discarding all changes and undo history. The buffer is restored to its original on-disk state. Cursor moves to the top. Not undoable. |
| `ai` | Open the AI assistant overlay **with file context**. The current file content (or visually selected lines) is sent along with your query to ChatGPT (gpt-4o). Requires `set ai-apikey=sk-...` in config. See **AI Assistant** section below for full details. |
| `ai-query` | Open the AI assistant overlay **without file context**. No file content is sent -- use this for general questions, documentation lookups, or anything not related to the current file. See **AI Assistant** section below for full details. |
| `ln` | Show line numbers in the left gutter of the current pane. Also accepts `linenumbers` or `line`. |
| `ln off` | Hide line numbers. Also accepts `linenumbers off` or `line off`. |
| `scope` | Toggle brace/function scope guide lines (Go/C files). |
| `scope off` | Hide scope guide lines. |
| `scope on` | Show scope guide lines. |
| `trim` | Remove trailing spaces and tabs from all lines in the current buffer. |
| `hex` | Toggle hex display for the current line. When on, the line under the cursor is shown as hex bytes (e.g. `48 65 6c 6c 6f`) instead of text. |
| `hex off` | Show the current line as normal text. |
| `hex on` | Show the current line in hex. |
| `themes` | Open the theme picker overlay. Use arrow keys to select a theme, Enter to apply, Escape to cancel. |
| `help` | Display the built-in help page with all commands. Press Enter or Escape to close the help page. |
| Escape | Cancel command mode and return to normal editing. |
| Tab | Command completion. If one match, auto-completes. If multiple, shows a popup list growing upward. Use Arrow Up/Down to navigate, Enter or Tab to accept, Escape to dismiss. Typing further filters the list. |

#### Examples: Command Mode with Ctrl-B

**Search for a pattern (case-sensitive):**

```
1. Press Ctrl-B                -> status bar bumps up, ":" prompt appears
2. Type /TODO                  -> press Enter
3. Cursor jumps to first "TODO" match.
4. Press Ctrl-X                -> jumps to the next match
5. Press Ctrl-X again          -> wraps around to the top if needed
6. Press Shift+Ctrl-X          -> jumps to the previous match
```

**Search for a pattern (case-insensitive):**

```
1. Press Ctrl-B
2. Type \error                 -> press Enter
3. Matches "error", "Error", "ERROR", etc.
```

**Save the current file with a new name:**

```
1. Press Ctrl-B
2. Type save/backup/myfile.txt -> press Enter
3. Status bar shows "Saved: backup/myfile.txt"
```

**Grep across your project files:**

```
1. Press Ctrl-B
2. Type grep -rni handleKey *.go  -> press Enter
3. Overlay shows all matches with filenames and line numbers.
4. Use Up/Down to browse results, Enter to open the file at that line.
5. Press Escape to close the grep overlay.
```

**Toggle line numbers on:**

```
1. Press Ctrl-B
2. Type ln                     -> press Enter
3. Line numbers appear in the left gutter.
4. To turn off: Ctrl-B, type ln off, press Enter.
```

**Run a shell command and view its output:**

```
1. Press Ctrl-B
2. Type !ls -la                -> press Enter
3. An overlay appears showing the directory listing.
4. Scroll with Up/Down or Page Up/Down, Escape to close.
```

**Run a shell command and insert its output into the buffer:**

```
1. Position cursor where you want the output inserted.
2. Press Ctrl-B
3. Type insert !date           -> press Enter
4. The current date/time is inserted below the cursor line.
5. Status bar shows "Inserted 1 lines from: date"
```

**Insert the output of a complex command:**

```
1. Press Ctrl-B
2. Type insert !cat /etc/hostname   -> press Enter
3. The contents of /etc/hostname are inserted at the cursor.
```

**Fuzzy search in the current buffer:**

```
1. Press Ctrl-B
2. Type %hndlkey               -> press Enter
3. Cursor jumps to the first line containing "h", "n", "d", "l", "k", "e", "y"
   in that order (e.g. "func handleKey(ev ..." matches).
4. Press Ctrl-X to jump to the next fuzzy match (wraps around).
5. Press Shift+Ctrl-X to jump to the previous fuzzy match.
```

**Replace with confirmation:**

```
1. Press Ctrl-B
2. Type replace foo bar confirm    -> press Enter
3. Cursor jumps to first "foo", highlighted.
4. Press y to replace with "bar", or n to skip.
5. Repeats for each occurrence. Press Esc to stop early.
6. Status bar shows "3 replaced" when done.
```

**Replace all without confirmation:**

```
1. Press Ctrl-B
2. Type replace foo bar ALL        -> press Enter
3. All occurrences of "foo" are replaced with "bar" instantly.
4. Status bar shows "12 replaced".
```

**Ask the AI assistant about your code (with file context):**

```
1. Add 'set ai-apikey=sk-...' to ~/.config/moe/moe.cnf
2. Open a file in moe.
3. Press Ctrl-B
4. Type ai                    → press Enter
5. AI overlay opens with a prompt. Your file content is sent as context.
6. Type: "Add error handling to this function" → press Enter
7. AI response appears in a scrollable overlay.
8. Press 'a' to apply the largest code block to your buffer.
9. Press 'r' to refine -- ask a follow-up question while keeping
   the full conversation history.
10. Press Esc to close.
```

**Ask the AI a general question (no file context):**

```
1. Press Ctrl-B
2. Type ai-query              → press Enter
3. AI overlay opens. No file content is sent.
4. Type: "What does O_NONBLOCK do?" → press Enter
5. Response appears in a scrollable overlay.
6. Press 'r' to ask a follow-up, or Esc to close.
```

**Use AI on a specific selection:**

```
1. Press Ctrl-E, then V       → visual line select mode
2. Select the lines you want the AI to work on
3. Press Ctrl-B, type ai      → press Enter
4. Only the selected lines are sent as context (not the full file).
5. Type your query and press Enter.
6. Press 'a' to apply -- the selected lines are replaced with the
   AI's code output.
```

**Switch themes:**

```
1. Press Ctrl-B
2. Type themes                 -> press Enter
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

## AI Assistant

moe integrates with ChatGPT (gpt-4o) for code assistance directly inside the editor. The AI overlay supports multi-turn conversations so you can refine your requests without losing context.

### Setup

Add your OpenAI API key to `~/.config/moe/moe.cnf`:

```
set ai-apikey=sk-...
```

### Two modes

| Command | Context sent | Best for |
|---|---|---|
| `ai` | Current file content (or visually selected lines) | Code editing, refactoring, explaining code in context |
| `ai-query` | None | General questions, documentation lookups, language syntax |

Both are accessed via command mode (`Ctrl-B`).

### AI overlay keys

**While typing your query (input phase):**

| Key | Action |
|---|---|
| Enter | Send the query to ChatGPT |
| Arrow keys | Navigate within the input text |
| Ctrl-Up / Ctrl-Down | Multi-line input navigation |
| Escape (press twice) | Close the AI overlay |

**While viewing the response:**

| Key | Action |
|---|---|
| `r` | **Refine** -- return to the input phase with the full conversation history preserved. Ask a follow-up question and the AI will have context from all prior exchanges. |
| `a` | **Apply** -- insert the largest code block from the response into the buffer. If you opened AI with a visual selection, the selected lines are replaced; otherwise the code is inserted at the cursor. |
| Up / Down | Scroll the response |
| Page Up / Page Down | Scroll faster |
| Escape | Close the AI overlay |

### Keeping a dialog going (AI mode with context)

The key to a multi-turn AI conversation is the **refine** feature (`r` key):

1. Open the AI overlay (`Ctrl-B ai` or `Ctrl-B ai-query`).
2. Type your first question and press Enter. The AI responds.
3. Press `r` to refine. The prompt reappears, but the conversation history is preserved.
4. Type a follow-up question (e.g., "Now add error handling" or "Make it concurrent"). Press Enter.
5. The AI sees the entire conversation -- your original question, its first answer, and your follow-up -- and responds in context.
6. Repeat step 3-5 as many times as needed. Each round adds to the history.
7. Press `a` at any point to apply the latest code block, or Escape to close.

This lets you iteratively build up a solution without re-explaining the problem each time.

### Using AI with visual selection

You can narrow the AI's focus to specific lines:

1. Press `Ctrl-E V` to enter visual line select mode.
2. Select the lines you want the AI to work on.
3. Press `Ctrl-B`, type `ai`, press Enter.
4. Only the selected lines are sent as context (not the entire file).
5. When you press `a` to apply, the selected lines are replaced with the AI's output.

This is useful for targeted refactoring -- select a function, ask the AI to optimize it, and apply the result directly.

## Split Screen

moe supports split-screen editing with two independent buffers, in either vertical (left/right) or horizontal (top/bottom) layout.

### Opening a split

- Press `Ctrl-V` for a **vertical split** (left/right). A vertical separator line appears in the middle. The right pane starts with an empty buffer.
- Press `Ctrl-H` for a **horizontal split** (top/bottom). A horizontal separator line appears. The bottom pane starts with an empty buffer.
- Alternatively, invoke moe with two filenames: `moe file1.txt file2.txt` opens directly in vertical split mode, or `moe -H file1.txt file2.txt` for horizontal split.

### Working in split mode

- **Vertical split:** Press `Ctrl-P` then `Left arrow` to switch to the left pane, or `Ctrl-P` then `Right arrow` for the right pane.
- **Horizontal split:** Press `Ctrl-P` then `Up arrow` to switch to the top pane, or `Ctrl-P` then `Down arrow` for the bottom pane.
- With mouse enabled (`mouse on`), click in an inactive pane to switch to it and position the cursor.
- The status bar at the bottom of the screen always belongs to the **active** pane. The inactive pane has no status bar.
- All editing commands (`Ctrl-S`, `Ctrl-F`, `Ctrl-B`, etc.) apply to the active pane.

### Closing a pane

- Press `Ctrl-W` to close the active pane. If the buffer has unsaved changes, moe prompts "Pane changed. Save? (y/n/c)".
- When the second pane is closed, the remaining pane takes the full screen.
- In single-pane mode, `Ctrl-W` behaves the same as `Ctrl-Q` (quit).

## Brace Scope Guides

When editing **Go** or **C** files (`.go`, `.c`, `.h`, `.cpp`, `.cc`, `.cxx`, `.hpp`), moe displays faint gray guide lines on the left edge of the editor. These guides show which `{` `}` brace pairs enclose the current cursor position:

- **Multiple nesting levels** are shown simultaneously -- deeper scopes appear slightly brighter.
- A top marker on the line containing the opening `{`.
- A bottom marker on the line containing the closing `}`.
- A vertical bar on continuation lines between the braces.
- Guides update automatically as you move the cursor or edit the buffer.
- Up to 4 nesting levels are displayed to avoid consuming too much horizontal space.
- Results are cached per buffer edit version for performance.
- To turn scope guides off or on: press `Ctrl-B`, type `scope off` or `scope on`, then Enter. Type `scope` alone to toggle.

```
|  func main() {
|| if x > 0 {
||     doSomething()
|| }
|  }
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

moe ships with 6 built-in themes:

| Theme | Description |
|---|---|
| `dark` | Default dark theme with a Catppuccin-inspired color palette. |
| `solarized-dark` | Solarized Dark -- warm muted tones on a dark blue-green background. |
| `monokai` | Monokai -- vibrant colors on a near-black background, popular in Sublime Text. |
| `gruvbox` | Gruvbox Dark -- retro-groove palette with warm earthy tones. |
| `nord` | Nord -- arctic, north-bluish color palette with cool tones. |
| `dracula` | Dracula -- dark theme with vibrant purples, pinks, and greens. |

To switch themes at runtime, press `Ctrl-B` to enter command mode, then type `themes` and press Enter. A theme picker overlay appears where you can navigate with the arrow keys and press Enter to select a theme. The change takes effect immediately. An asterisk (`*`) marks the currently active theme.

The selected theme is automatically saved to `~/.config/moe/moe.cnf` and restored on next launch.

## Mouse Support

moe supports optional mouse interaction. Mouse capture is **off by default** to preserve native terminal selection (Cmd-C on macOS, Ctrl-Shift-C on Linux).

### Enabling mouse

- In the editor: press `Ctrl-B`, type `mouse on`, press Enter.
- In config (`~/.config/moe/moe.cnf`): add `set mouse=on`.
- Toggle with `Ctrl-B` then `mouse`.

### Mouse actions

| Action | Effect |
|---|---|
| Left click | Position cursor at click location |
| Click + drag | Select text (visual character mode) |
| Click on line number | Select entire line (visual line mode) |
| Scroll wheel up/down | Scroll buffer by 3 lines |
| Click inactive pane | Switch to that pane and position cursor |
| Click in file browser | Select the clicked entry |
| Click in grep results | Select the clicked result |
| Click in theme picker | Select the clicked theme |

### Native terminal selection

When mouse capture is on, hold **Shift** (Linux) or **Option** (macOS) while clicking to use native terminal selection for copy/paste.

## WordStar Mode

moe supports classic WordStar key bindings for users who grew up with WordStar, Turbo Pascal, or Borland C++. WordStar mode is **off by default**.

### Enabling WordStar mode

- In the editor: press `Ctrl-B`, type `wordstar on`, press Enter.
- In config (`~/.config/moe/moe.cnf`): add `set wordstar=on`.
- Toggle with `Ctrl-B` then `wordstar`.

### Cursor diamond

| Key | Action |
|---|---|
| `Ctrl-E` / `Ctrl-X` | Cursor up / down |
| `Ctrl-S` / `Ctrl-D` | Cursor left / right |
| `Ctrl-A` / `Ctrl-F` | Word left / right |
| `Ctrl-R` / `Ctrl-C` | Page up / down |

### Editing keys

| Key | Action |
|---|---|
| `Ctrl-G` | Delete character at cursor |
| `Ctrl-T` | Delete word right |
| `Ctrl-Y` | Delete entire line |
| `Ctrl-N` | Insert new line |
| `Ctrl-U` | Undo |
| `Ctrl-L` | Find next match |
| `Ctrl-W` / `Ctrl-Z` | Scroll up / down one line |

### Quick commands (Ctrl-Q prefix)

Press `Ctrl-Q` then a second key:

| Second key | Action |
|---|---|
| `S` / `D` | Home / End of line |
| `R` / `C` | Top / Bottom of file |
| `F` | Find (opens search prompt) |
| `A` | Replace (opens replace prompt) |
| `Y` | Delete to end of line |

### Block/File commands (Ctrl-K prefix)

Press `Ctrl-K` then a second key:

| Second key | Action |
|---|---|
| `S` / `Q` / `D` | Save / Quit / Save+Quit |
| `B` | Start block selection |
| `V` | Paste below cursor |
| `Y` | Delete line |
| `F` | File browser |
| `U` | Redo |

### Keys unchanged in WordStar mode

`Ctrl-B` (command mode) and `Ctrl-P` (pane switch) work the same in both modes. Arrow keys, Home, End, Page Up/Down, and Escape prefix commands are also unchanged.

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
| `theme` | `dark`, `solarized-dark`, `monokai`, `gruvbox`, `nord`, `dracula` | `dark` | Color theme |
| `linenumbers` | `on`, `off` | `off` | Show line numbers in the gutter |
| `mouse` | `on`, `off` | `off` | Enable mouse click, drag, and scroll support |
| `wordstar` | `on`, `off` | `off` | Use classic WordStar key bindings |
| `city` | Any city name (e.g. `Milan`, `New York`, `Tokyo`) | *(none)* | City used for weather lookups via [wttr.in](https://wttr.in) |
| `statusbar` | Comma-separated: `time`, `weather` | *(none)* | Extra information to show on the right side of the status bar |
| `ai-apikey` | OpenAI API key (e.g. `sk-...`) | *(none)* | API key for the AI assistant (`Ctrl-B ai`). Uses the gpt-4o model. |

#### Status bar: time and weather

When `set statusbar=time` is configured, the local computer time (HH:MM) is shown on the right side of the status bar, updating every second.

When `set statusbar=weather` is configured along with `set city=Milan`, moe will:

1. Wait 30 seconds after startup (to avoid slowing down the editor launch).
2. Check for internet connectivity.
3. If online, fetch the current temperature and weather from [wttr.in](https://wttr.in) for the configured city.
4. Display the weather (e.g. `+22C`) on the right side of the status bar.

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

## Version

Check the current version:

```bash
./moe --version
```


## License

All rights reserved. Copyright 2026 by moshix.
