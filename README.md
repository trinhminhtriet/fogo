# 🧹 fogo

```text
  __
 / _|  ___    __ _   ___
| |_  / _ \  / _` | / _ \
|  _|| (_) || (_| || (_) |
|_|   \___/  \__, | \___/
             |___/

```

**fogo** is a fast, minimal terminal file explorer designed to streamline your navigation workflow. Instead of relying on repeated use of `cd` and `ls` to browse your filesystem, fogo provides a simple and efficient Text User Interface (TUI) for navigating folders.

## ✨ Features

- **Fast Navigation**: Quickly navigate directories with minimal keystrokes.
- **Minimal and Intuitive**: Focuses solely on folder navigation—no distractions like file management operations (create, delete, or rename).
- **Type-Ahead Search**: Inspired by GUI file managers, fogo lets you search folders dynamically as you type.

## What fogo Is Not

fogo is not a file manager. It cannot manipulate files or directories—its purpose is strictly navigation.

## Why fogo?

Efficient filesystem navigation is essential for developers and power users. fogo simplifies this process, helping you move around your terminal quickly without breaking focus.

## 🚀 Installation

To install **fogo**, simply clone the repository and follow the instructions below:

```bash
git clone git@github.com:trinhminhtriet/fogo.git
cd fogo

cargo build --release
cp ./target/release/fogo /usr/local/bin/
```

## 💡 Usage

### Step 2: Configure your shell to `cd` using `fogo`

`fogo` only prints a folder when it exits. To make your shell actually `cd` to this folder, you have to define a function or alias, since the working directory cannot be changed by a subprocess. See instructions for your shell below.

<details>
<summary>Bash/Zsh</summary>

Put this in your `.bashrc` or `.zshrc`:

```sh
fogo() {
    local result=$(command fogo "$@")
    [ -n "$result" ] && cd -- "$result"
}
```

</details>

<details>
<summary>fish</summary>

Put this in your `config.fish`:

```sh
function fogo
    set --local result (command fogo $argv)
    [ -n "$result" ] && cd -- "$result"
end
```

</details>

<details>
<summary>Xonsh</summary>

Put this in your `.xonshrc` (Xonsh v0.10. or newer is required):

```py
def _fogo(args):
    result = $(fogo @(args)).strip()
    if result:
        cd @(result)

aliases["fogo"] = _fogo
```

</details>

<details>
<summary>Nushell</summary>

Put this in your `config.nu` (Nushell 0.88.0 or newer is required):

```nushell
def --wrapped --env fogo [...args]: {
    let result = ( ^fogo ...$args )
    if $result != "" {
        cd $result
    }
}
```

</details>

<details>
<summary>PowerShell</summary>

Put this in your `$PROFILE`:

```powershell
function Invoke-Tere() {
    $result = . (Get-Command -CommandType Application fogo) $args
    if ($result) {
        Set-Location $result
    }
}
Set-Alias fogo Invoke-Tere
```

</details>

<details>
<summary>Windows Command Prompt (CMD)</summary>

Put this in a batch script file called `fogo.bat` in a folder included in your `PATH` environment variable such as `C:\Windows`:

```batch
@echo off

rem set the location/path of the fogo executable here...
SET TereEXE=C:\path\to\fogo.exe

FOR /F "tokens=*" %%a in ('%TereEXE% %*') do SET OUTPUT=%%a
IF ["%OUTPUT%"] == [""] goto :EOF
cd %OUTPUT%
```

Note that if you want to make `fogo` work with _both_ PowerShell and CMD, you should _not_ put `fogo.exe` to a location that is in your `PATH`, because then the `.exe` will be run instead of the `.bat`. Place `fogo.exe` somewhere that is not in your `PATH`, and use the full path to the exe in both the `.bat` file and in the PowerShell `$PROFILE`.

</details>

If `fogo` is not in your `PATH`, use an absolute path to the fogo binary in your shell config file. For example, for Bash/Zsh, you would need to replace `local result=$(command fogo "$@")` with `local result=$(/path/to/fogo "$@")`, or for PowerShell, replace `(Get-Command -CommandType Application fogo)` with `C:\path\to\fogo.exe`.

### Keyboard shortcuts

`fogo` has the following keyboard shortcuts by default:

|              Description               |                                                         Default shortcut(s)                                                          |        Action name        |
| :------------------------------------: | :----------------------------------------------------------------------------------------------------------------------------------: | :-----------------------: |
|      Enter directory under cursor      | <kbd>Enter</kbd> or <kbd>→</kbd> or <kbd>Alt</kbd>-<kbd>↓</kbd> or <kbd>Alt</kbd>-<kbd>l</kbd> or if not searching, <kbd>Space</kbd> |        `ChangeDir`        |
|         Go to parent directory         | <kbd>←</kbd> or <kbd>Alt</kbd>-<kbd>↑</kbd> or <kbd>Alt</kbd>-<kbd>h</kbd> or if not searching, <kbd>Backspace</kbd> or <kbd>-</kbd> |     `ChangeDirParent`     |
|          Go to home directory          |                    <kbd>~</kbd> or <kbd>Ctrl</kbd>-<kbd>Home</kbd> or <kbd>Ctrl</kbd>-<kbd>Alt</kbd>-<kbd>h</kbd>                    |      `ChangeDirHome`      |
|          Go to root directory          |                                             <kbd>/</kbd> or <kbd>Alt</kbd>-<kbd>r</kbd>                                              |      `ChangeDirRoot`      |
|             Move cursor up             |                                             <kbd>↑</kbd> or <kbd>Alt</kbd>-<kbd>k</kbd>                                              |        `CursorUp`         |
|            Move cursor down            |                                             <kbd>↓</kbd> or <kbd>Alt</kbd>-<kbd>j</kbd>                                              |       `CursorDown`        |
|      Move cursor up by one screen      |                          <kbd>Page Up</kbd> or <kbd>Ctrl</kbd>-<kbd>u</kbd> or <kbd>Alt</kbd>-<kbd>u</kbd>                           |     `CursorUpScreen`      |
|     Move cursor down by one screen     |                         <kbd>Page Down</kbd> or <kbd>Ctrl</kbd>-<kbd>d</kbd> or <kbd>Alt</kbd>-<kbd>d</kbd>                          |    `CursorDownScreen`     |
|         Move cursor to the top         |                                            <kbd>Home</kbd> or <kbd>Alt</kbd>-<kbd>g</kbd>                                            |        `CursorTop`        |
|       Move cursor to the bottom        |                                    <kbd>End</kbd> or <kbd>Alt</kbd>-<kbd>Shift</kbd>-<kbd>g</kbd>                                    |      `CursorBottom`       |
|   Erase a character from the search    |                                                  <kbd>Backspace</kbd> if searching                                                   |     `EraseSearchChar`     |
|            Clear the search            |                                                     <kbd>Esc</kbd> if searching                                                      |       `ClearSearch`       |
|          Toggle filter search          |                                                     <kbd>Alt</kbd>-<kbd>f</kbd>                                                      | `ChangeFilterSearchMode`  |
|      Change case sensitivity mode      |                                                     <kbd>Alt</kbd>-<kbd>c</kbd>                                                      | `ChangeCaseSensitiveMode` |
|         Change gap search mode         |                                                     <kbd>Ctrl</kbd>-<kbd>f</kbd>                                                     |   `ChangeGapSearchMode`   |
|          Change sorting mode           |                                                     <kbd>Alt</kbd>-<kbd>s</kbd>                                                      |     `ChangeSortMode`      |
|       Refresh current directory        |                                                     <kbd>Ctrl</kbd>-<kbd>r</kbd>                                                     |     `RefreshListing`      |
|            Show help screen            |                                                             <kbd>?</kbd>                                                             |          `Help`           |
|              Exit `fogo`               |                                            <kbd>Esc</kbd> or <kbd>Alt</kbd>-<kbd>q</kbd>                                             |          `Exit`           |
|    Enter directory and exit `fogo`     |                                 <kbd>Alt</kbd>-<kbd>Enter</kbd> or <kbd>Ctrl</kbd>-<kbd>Space</kbd>                                  |    `ChangeDirAndExit`     |
| Exit `fogo` without changing directory |                                                     <kbd>Ctrl</kbd>-<kbd>c</kbd>                                                     |      `ExitWithoutCd`      |

Some of the shortcuts starting with <kbd>Alt</kbd> should be familiar to Vim users.

### **Customizing Keyboard Shortcuts**

Keyboard shortcuts in `fogo` can be customized using the `--map` (or `-m`) command-line option. Mappings follow these formats:

- **Basic Mapping**: `--map key-combination:action`
- **Contextual Mapping**: `--map key-combination:context:action`
- **Key Combination (`key-combination`)**: Specifies a key or combination, e.g., `ctrl-x`.
- **Action (`action`)**: Represents a predefined operation such as `Exit` or `ChangeDir` (refer to the `--help` output or documentation for a full list).
- **Context (`context`)**: Optional, determines when the mapping applies, e.g., `Searching` or `NotSearching`.

To **remove a mapping**, specify `None` as the action:

```bash
--map key-combination:None
```

You can define multiple mappings by:

1. Repeating the `--map` option:
   ```bash
   --map combination1:action1 --map combination2:action2
   ```
2. Using a comma-separated list:
   ```bash
   --map combination1:action1,combination2:action2
   ```

For additional examples and a full list of actions, use the `--help` option.

---

### **Searching**

`fogo` enables fast folder navigation with its incremental search feature. Simply type to begin searching for items in the current directory. Matching results are highlighted dynamically, and navigation is limited to those matches. Key behaviors include:

- **Real-time Feedback**: Displays the search query and number of matches at the bottom of the screen.
- **Single Match Optimization**: If only one folder matches, it is auto-highlighted, and the working directory updates to it.
- **Stopping Search**: Press <kbd>Esc</kbd> or use <kbd>Backspace</kbd> to clear the search query.

**Default Behavior**:

- `fogo` searches **folders only**, as it cannot operate on files. Use the `--files` option to modify this behavior.
- **Case Sensitivity**: By default, "smart case" is enabled:
  - Lowercase queries ignore case (case-insensitive).
  - Uppercase queries are case-sensitive.  
    Customize this behavior with `--ignore-case`, `--case-sensitive`, or toggle it with <kbd>Alt</kbd>-<kbd>c</kbd>.

**Search Modes**:

- **Gap Search** (default): Matches folder names even if query characters are non-consecutive. For instance, `dt` matches both `DeskTop` and `DocumenTs`.
- **Gap Search Variants**:
  - `--gap-search-anywhere`: Allows matches even if the first query character doesn’t align with the folder name’s start.
  - Disable gap search with `--normal-search` or `--normal-search-anywhere` to enforce strict matching of consecutive characters.

Toggle search modes dynamically using <kbd>Ctrl</kbd>-<kbd>f</kbd>.

---

### **Mouse Navigation**

While `fogo` is optimized for keyboard use, it supports mouse navigation when enabled via `--mouse=on`.

- **Left Click**: Enter a folder.
- **Right Click**: Move to the parent folder.

Mouse navigation is disabled by default for compatibility.

---

### **CLI Options**

`fogo` offers various options to customize its behavior:

- `--help`, `-h`: Display a help summary and available options (output to `stderr`).
- `--version`, `-V`: Show the current version of `fogo`.
- `--filter-search`, `-f` / `--no-filter-search`, `-F`: Choose whether to show only matching items (`--filter-search`) or all items (`--no-filter-search`) while searching.
- `--files`, `-l ignore/hide/match`: Configure file handling during search:
  - `ignore` (default): Searches folders only.
  - `hide`: Displays folders but hides files.
  - `match`: Matches both files and folders (useful for verifying file presence).
- `--smart-case`, `-S` / `--ignore-case`, `-i` / `--case-sensitive`, `-s`: Configure case sensitivity.
- `--gap-search`, `-g` / `--gap-search-anywhere`, `-G` / `--normal-search`, `-n` / `--normal-search-anywhere`, `-N`: Customize search behavior (see _Searching_ above).
- `--sort name/created/modified`: Change sorting criteria for listed items.
- `--autocd-timeout`: Automatically navigate to a matching folder after a specified timeout in milliseconds (use `off` to disable).
- `--history-file`: Define the path to save folder navigation history (default: `$CACHE_DIR/fogo/history.json`). Set to `''` to disable saving history.
- `--mouse=on/off`: Enable or disable mouse navigation.

For options with opposing flags (e.g., `--filter-search` vs. `--no-filter-search`), the last specified option takes precedence. This allows default settings in shell configuration files while permitting overrides during runtime.

For more detailed information, refer to `--help`.

## 🤝 How to contribute

We welcome contributions!

- Fork this repository;
- Create a branch with your feature: `git checkout -b my-feature`;
- Commit your changes: `git commit -m "feat: my new feature"`;
- Push to your branch: `git push origin my-feature`.

Once your pull request has been merged, you can delete your branch.

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
