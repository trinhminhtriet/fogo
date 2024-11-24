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

Running the below command will globally install the `fogo` binary.

```bash
cargo install fogo
```

Optionally, you can add `~/.cargo/bin` to your PATH if it's not already there

```bash
echo 'export PATH="$HOME/.cargo/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

## 💡 Usage

Explore fogo today and make your terminal navigation faster and easier!

```sh
fogo
```

## 🤝 How to contribute

We welcome contributions!

- Fork this repository;
- Create a branch with your feature: `git checkout -b my-feature`;
- Commit your changes: `git commit -m "feat: my new feature"`;
- Push to your branch: `git push origin my-feature`.

Once your pull request has been merged, you can delete your branch.

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
