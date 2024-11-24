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

#### Customizing keyboard shortcuts

All of the keyboard shortcuts listed above can be customized using the `--map` (or `-m`) CLI option. Keyboard mappings can be either of the form `--map key-combination:action` or `--map key-combination:context:action`, where `key-combination` is a key combination, such as `ctrl-x`, `action` is a valid action name (for example `Exit` or `ChangeDir`, see the table above or `--help` for a full list of actions), and the optional `context` specifies the context in which the mappling applies (for example `Searching` and `NotSearching`, see `--help`). To remove a mapping, use `--map key-combination:None`. Multiple mappings can be made by providing `--map` multiple times, or by using a comma-separated list of mappings: `--map combination1:action1,combination2:action2`.

For further details and examples, see the output of `--help`.

### Searching

To search for an item in the current folder, just type some letters. `fogo` will incrementally highlight all folders that match the search query.

While searching, moving the cursor up or down jumps between only the items that match the search. The search query, as well as the number of matching items is shown at the bottom of the screen.

If only one folder matches your current search, `fogo` will highlight it, and change the working directory to that folder. This way you can navigate folders very quickly.

To stop searching, press <kbd>Esc</kbd> or erase all search characters by pressing <kbd>Backspace</kbd>.

Note that by default, `fogo` searches only folders and not files, since `fogo` cannot do anything with files. This can be changed with the `--files` option. For further details, see below, or check the output of `--help`.

By default, the searching uses "smart case", meaning that if the query contains only lowercase letters, case is ignored, but if there are uppercase letters, the search is case sensitive. This can be changed with the `--ignore-case` and `--case-sensitive` options, or with the keyboard shortcut <kbd>Alt</kbd>-<kbd>c</kbd> by default.

Additionally, in the default search mode, "gap search" (sometimes also known as fuzzy search) is enabled. This means that the search matches any folder name as long as it starts with the same character as the search query, and contains the rest of the query characters, even if there are other characters between them. For example, searching for `dt` would match both `DeskTop` and `DocumenTs`. With the `--gap-search-anywhere` option, the first character of the query doesn't have to match the first character of a folder/file name. The gap search can be disabled with the `--normal-search` and `--normal-search-anywhere` options, which only allow matching consecutive characters, either from the start or anywhere within the folder/file name, respsectively. The gap search behavior can also be changed with the keyboard shortcut <kbd>Ctrl</kbd>-<kbd>f</kbd> by default. See `--help` for details.

### Mouse navigation

Although `fogo` is mainly keyboard-focused, it is also possible to navigate using the mouse. To maximize compatibility, mouse support is off by default, and has to be enabled with the option `--mouse=on`. With the mouse enabled, you can change to a folder by clicking on it, and move to the parent folder by right-clicking.

### CLI options

You can adjust the behavior of `fogo` by passing the following CLI options to it:

- `--help` or `-h`: Print a short help and all CLI options. Note that the output goes to stderr, to not interfere with `cd` ing in the shell functions defined during the setup.
- `--version` or `-V`: Print the version of `fogo`. This also goes to stderr.
- `--filter-search` or `-f` / `--no-filter-search` or `-F`: If `--filter-search` is set, show only items that match the current search query in the listing. Otherwise all items are shown in the listing while searching (this is the default behavior).
- `--files` or `-l ignore` / `hide` / `match` (or `i` / `h` / `m`): How to handle files while searching. If `ignore` (the default), only folders are searched / matched. If `hide`, files are hidden and only folders shown and matched, and if `match`, both files and folders are matched. Note that currently `fogo` cannot do anything with files, so searching for a file name with `--files=match` is mostly only useful for checking whether a file can be found in the current folder.
- `--smart-case` or `-S` / `--ignore-case` or `-i` / `--case-sensitive` or `-s`: Set the case sensitivity mode. The default mode is smart case, which is case insensitive if the query contains only lowercase letters and case sensitive otherwise.
- `--gap-search` or `-g` / `--gap-search-anywhere` or `-G` / `--normal-search` or `-n` / `--normal-search-anywhere` or `-N`: Configure whether to allow matches with gaps in them (see above).
- `--sort name` / `created` / `modified`: Change the sorting order of the listing.
- `--autocd-timeout` - If the current search matches only one folder, automatically change to that folder after this many milliseconds. Can also be set to `off`, which disables this behaviour.
- `--history-file`: To make browsing more convenient, `fogo` saves a history of folders you have visited to this file in JSON format. It should be an absolute path. Defaults to `$CACHE_DIR/fogo/history.json`, where `$CACHE_DIR` is `$XDG_CACHE_HOME` or `~/.cache`. Set to the empty string `''` to disable saving the history. Note that the history reveals parts of your folder structure if it can be read by someone else.
- `--mouse=on` or `--mouse=off`: Enable or disable navigating with the mouse. If enabled, you can left-click to enter folders and right-click to go to the parent folder. Off by default.

Some options have two or more versions that override each other (for example `--filter-search` and `--no-filter-search`). For such options, whichever is passed last wins. This way, you can have one option as the default in your shell's `rc` file, but you can sometimes manually override that option when running `fogo`.

## 🤝 How to contribute

We welcome contributions!

- Fork this repository;
- Create a branch with your feature: `git checkout -b my-feature`;
- Commit your changes: `git commit -m "feat: my new feature"`;
- Push to your branch: `git push origin my-feature`.

Once your pull request has been merged, you can delete your branch.

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
