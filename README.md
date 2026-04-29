# Helix Editor Configuration

This repository contains my configuration files for the Helix text editor. It is optimized for a keyboard-driven workflow and specifically tuned for writing, documentation, and worldbuilding using Typst.

## Overview

This setup enhances the default Helix experience with a few quality-of-life UI adjustments and a specialized language server setup for prose and markup. 

### Key Features

* **Intelligent Spellchecking:** Integrates the Rust-based `harper-ls` language server for fast, syntax-aware grammar and spellchecking.
* **Dual Language Servers:** Configures Helix to run `tinymist` and `harper-ls` simultaneously for `.typ` files. This ensures standard compiler errors and autocompletion remain active alongside the grammar linting.
* **Document Formatting:** Enforces an 80-character text width with a visual ruler and soft-wrapping for plain text and Typst files to keep documents readable.
* **UI & Keybinds:** Includes a custom, decluttered statusline and maps `jj` to quickly exit insert mode.

## Installation & Setup

When setting up a new machine, follow these steps to restore this configuration.

### 1. Install Dependencies

You will need the Helix editor, a Typst language server (`tinymist`), and Harper. 

Install Harper via Cargo:
```bash
cargo install harper-ls
```

Install Tinymist via Cargo:
*(Note: Because the main crates.io package is a library, you must install the CLI binary directly from their Git repository).*
```bash
cargo install --git [https://github.com/Myriad-Dreamin/tinymist](https://github.com/Myriad-Dreamin/tinymist) --locked tinymist-cli
```

### 2. Link Configuration Files

Helix looks for configuration files in `~/.config/helix/`. Instead of copying the files directly, create symbolic links from this repository so that future edits are automatically tracked by Git.

Run the following commands to remove the default static files (if they exist) and create the symlinks:
```bash
# Ensure the Helix config directory exists
mkdir -p ~/.config/helix

# Remove any existing default files
rm -f ~/.config/helix/languages.toml
rm -f ~/.config/helix/config.toml

# Create symlinks pointing to this repository
ln -s ~/Documents/repos/helix_config/languages.toml ~/.config/helix/languages.toml
ln -s ~/Documents/repos/helix_config/config.toml ~/.config/helix/config.toml
```

### 3. Verify Setup

To ensure Helix recognizes the new configuration and language servers, open your terminal and run:

```bash
hx --health typst
```

Check that both `tinymist` and `harper-ls` are listed and active. You can also open a file in Helix and type `:log-open` to view the server startup logs.

### Managing the Dictionary

Harper maintains local dictionaries to prevent custom worldbuilding terms, character names, or technical jargon from being flagged.

When hovering over a flagged word in Helix, press `Space + a` to open the Code Actions menu and add the word to your dictionary.

* **Global User Dictionary:** Stored at `~/.config/harper-ls/dictionary.txt`
* **Workspace Dictionary:** Create a `.harper-dictionary.txt` file at the root of a specific project
